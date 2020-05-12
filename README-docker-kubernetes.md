Docker Desktop for Windows CE:
https://hub.docker.com/editions/community/docker-ce-desktop-windows

docker version
Client: Docker Engine - Community
 Version:           19.03.8
 API version:       1.40
 Go version:        go1.12.17
 Git commit:        afacb8b
 Built:             Wed Mar 11 01:23:10 2020
 OS/Arch:           windows/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.8
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.17
  Git commit:       afacb8b
  Built:            Wed Mar 11 01:29:16 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.2.13
  GitCommit:        7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
 Kubernetes:
  Version:          v1.15.5
  StackAPI:         v1beta2

docker info
Client:
 Debug Mode: false

Server:
 Containers: 22
  Running: 20
  Paused: 0
  Stopped: 2
 Images: 17
 Server Version: 19.03.8
 Storage Driver: overlay2
  Backing Filesystem: <unknown>
  Supports d_type: true
  Native Overlay Diff: true
 Logging Driver: json-file
 Cgroup Driver: cgroupfs
 Plugins:
  Volume: local
  Network: bridge host ipvlan macvlan null overlay
  Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
 Swarm: inactive
 Runtimes: runc
 Default Runtime: runc
 Init Binary: docker-init
 containerd version: 7ad184331fa3e55e52b890ea95e65ba581ae3429
 runc version: dc9208a3303feef5b3839f4323d9beb36df0a9dd
 init version: fec3683
 Security Options:
  seccomp
   Profile: default
 Kernel Version: 4.19.76-linuxkit
 Operating System: Docker Desktop
 OSType: linux
 Architecture: x86_64
 CPUs: 2
 Total Memory: 1.919GiB
 Name: docker-desktop
 ID: IU22:UBRW:QCML:475S:6TI6:F4FH:KUSB:EUFB:POEK:UXR6:NQJ2:WLGB
 Docker Root Dir: /var/lib/docker
 Debug Mode: true
  File Descriptors: 143
  Goroutines: 135
  System Time: 2020-05-02T20:24:03.364169707Z
  EventsListeners: 3
 Registry: https://index.docker.io/v1/
 Labels:
 Experimental: false
 Insecure Registries:
  127.0.0.0/8
 Live Restore Enabled: false
 Product License: Community Engine

> kubectl version
Client Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.5", GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", GitTreeState:"clean", BuildDate:"2019-10-15T19:16:51Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"windows/amd64"}
Server Version: version.Info{Major:"1", Minor:"15", GitVersion:"v1.15.5", GitCommit:"20c265fef0741dd71a66480e35bd69f18351daea", GitTreeState:"clean", BuildDate:"2019-10-15T19:07:57Z", GoVersion:"go1.12.10", Compiler:"gc", Platform:"linux/amd64"}

Setting up Kubernetes Dashboard

kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v1.10.1/src/deploy/recommended/kubernetes-dashboard.yaml

kubectl proxy

Type this on the browser:
http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/


curl http://localhost:8001/api
{
  "kind": "APIVersions",
  "versions": [
    "v1"
  ],
  "serverAddressByClientCIDRs": [
    {
      "clientCIDR": "0.0.0.0/0",
      "serverAddress": "192.168.65.3:6443"
    }
  ]
}

To get token:

1)
kubectl -n kube-system describe secret default
copy after "token:"

kubectl config set-credentials docker-for-desktop --token=eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLXRrZ2tyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJlMzgxYzBlNy1lNzE1LTQ3ZDUtODlkOC1jOTE3MTVhNmVjYjMiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.o9iRkahPnR1uvO87O8N_w0B_0GXtUcJSy2OF8THV1GKHSlyRwVxU61j5meA3blY6WO7Grg7Yh1TcAA8Ztp1I9MXNWy0wlf7ETC7hM7Z3K7Y5Guc-hcjL-Y5KXc5PU76QNTUXQnU-6IRJ8QOEiUBcn7j6-4RpsvCwCkHp5ad2ZgjksjDfl4J9S5bu9uh-hiygqy_KJU8fIJnyxRMZpgq4yirZH_2X7d55inNRIsD0M5TNqur3xVOVER70-1aztWl1Bi62-fxC-QajVWaFjRth2zxfYnFBaY_b0DfwrgHq8ZIvkGMEXB6LD3RLAjiSyKiNkmL0eHkuFwbAelLR-RKA8w


Use this token to login:
eyJhbGciOiJSUzI1NiIsImtpZCI6IiJ9.eyJpc3MiOiJrdWJlcm5ldGVzL3NlcnZpY2VhY2NvdW50Iiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9uYW1lc3BhY2UiOiJrdWJlLXN5c3RlbSIsImt1YmVybmV0ZXMuaW8vc2VydmljZWFjY291bnQvc2VjcmV0Lm5hbWUiOiJkZWZhdWx0LXRva2VuLXRrZ2tyIiwia3ViZXJuZXRlcy5pby9zZXJ2aWNlYWNjb3VudC9zZXJ2aWNlLWFjY291bnQubmFtZSI6ImRlZmF1bHQiLCJrdWJlcm5ldGVzLmlvL3NlcnZpY2VhY2NvdW50L3NlcnZpY2UtYWNjb3VudC51aWQiOiJlMzgxYzBlNy1lNzE1LTQ3ZDUtODlkOC1jOTE3MTVhNmVjYjMiLCJzdWIiOiJzeXN0ZW06c2VydmljZWFjY291bnQ6a3ViZS1zeXN0ZW06ZGVmYXVsdCJ9.o9iRkahPnR1uvO87O8N_w0B_0GXtUcJSy2OF8THV1GKHSlyRwVxU61j5meA3blY6WO7Grg7Yh1TcAA8Ztp1I9MXNWy0wlf7ETC7hM7Z3K7Y5Guc-hcjL-Y5KXc5PU76QNTUXQnU-6IRJ8QOEiUBcn7j6-4RpsvCwCkHp5ad2ZgjksjDfl4J9S5bu9uh-hiygqy_KJU8fIJnyxRMZpgq4yirZH_2X7d55inNRIsD0M5TNqur3xVOVER70-1aztWl1Bi62-fxC-QajVWaFjRth2zxfYnFBaY_b0DfwrgHq8ZIvkGMEXB6LD3RLAjiSyKiNkmL0eHkuFwbAelLR-RKA8w



Reference:
https://collabnix.com/kubernetes-dashboard-on-docker-desktop-for-windows-2-0-0-3-in-2-minutes/