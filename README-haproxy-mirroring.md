# Setup Haproxy in Ubutu


#### haproxy.cfg

global
   log stdout local0
   maxconn 4096

defaults
   log global
   mode http
   balance leastconn
   retries 3
   option httpchk GET /health HTTP/1.0\r\nHost:\ localhost
   option http-server-close
   option dontlognull
   option httplog
   timeout connect 30ms
   timeout check 1000ms
   timeout client 50000ms
   timeout server 50000ms

frontend http-in
   bind *:8000
   mode http
   option http-buffer-request
   filter spoe engine mirror config /home/ksomalin/haproxy/mirrorconf/mirror.cfg

   option httplog

   acl is_one path_beg -i /sc/one
   acl is_two path_beg -i /sc/two

   use_backend serverone if is_one
   use_backend servetwo if is_two

   default_backend server

backend server
   option forwardfor
   http-request set-header X-Forwarded-port %[dst_port]
   option httpchk HEAD / HTTP/1.1\r\nHost:localhost
   server s1 127.0.0.1:9000 maxconn 32

backend serverone
   option forwardfor
   http-request set-header X-Forwarded-host %[req.hdr(Host)]
   server s2 127.0.0.1:9001 maxconn 32

backend servertwo
   option forwardfor
   server s3 127.0.0.1:9002 maxconn 32

backend mirroragents
   mode tcp
   balance roundrobin
   timeout connect 5s
   timeout server 5s
   server agent1 localhost:12345

listen stats
   bind *:9080
   stats enable
   stats uri /stats


##### Mirror agent conf

mirror.conf
[mirror]
spoe-agent mirror
    log global
    messages mirror
    use-backend mirroragents
    timeout hello 500ms
    timeout idle 5s
    timeout processing 5s

spoe-message mirror
    args arg_method=method arg_path=url arg_ver=req.ver arg_hdrs=req.hdrs_bin arg_body=req.body
    event on-frontend-http-request

#### Mirror receiver
SPOA Agent:

git clone https://github.com/haproxytech/spoa-mirror

FROM ubuntu:bionic
RUN apt update && apt install -y curl autoconf automake build-essential git libcurl4-openssl-dev libev-dev libpthread-stubs0-dev pkg-config
WORKDIR /build
COPY ./spoa-mirror .
RUN ./scripts/bootstrap
RUN ./configure --enable-debug
RUN make all
RUN ./src/spoa-mirror --version
ARG MIRROR_URL=http://localhost:9080/
ENV MIRROR_URL $MIRROR_URL
RUN echo $MIRROR_URL
ENTRYPOINT ./src/spoa-mirror --num-workers 1 --runtime 0 -u $MIRROR_URL --logfile /var/log/haproxy-mirror.log

docker build -t containers.cisco.com/ccw_renewal/haproxymirroragent:2 --build-arg MIRROR_URL=http://localhost:8080/sc/one .

docker push containers.cisco.com/ccw_renewal/haproxymirroragent:2

# Setup backend server - use nodejs its easy to start the backend server

For example, this simple server.js script will serve a small text page, including the serving address and port, on the machine's ports 9000-9002:

nodejs

var http = require('http');

function serve(ip, port){
http.createServer(function (req, res){
res.writeHead(200, {'Content-Type': 'text/plain'});
res.write(JSON.stringify(req.headers));
res.end("\n Hello World, There's no place like "+ip+":"+port+"\n");
}).listen(port, ip);
console.log('Server running at http://'+ip+':'+port+'/');
}

// Create three servers for// the load balancer, listening on any// network on the following three ports 
serve('0.0.0.0', 9000);
serve('0.0.0.0', 9001);
serve('0.0.0.0', 9002);

#### To start
nohup node server.js & > out.log