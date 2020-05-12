# Wiremock

#### Used for mocking the API's

#### How to use

1) Down load wiremock-standalone-<version>.jar


2) To Start capture the request and response, it will be per URL

example:
EndPoint: google.com/api/users
Mocking URL : localhost:9999/api/users

java -jar wiremock-standalone-2.26.2.jar --port 9999 -proxy-all="https://google.com" --record-mappings --verbose

POST http://localhost:9999/api/users
localhost --> will proxy to google.com and hit google.com/api/users

Request and Response (mappings(Request) / __files(Response)) folder will be created and input / output is captured in the corresponding folders.

2) Stop the wiremock standalone and start with below script

java -jar wiremock-standalone-2.26.2.jar --port 9999 (now we are not doing the proxy, just mocking the api which are already captured)

3) Hit mock api to get the response instead of real API.

Note: URL, input are considered while mocking the request, it should be exactly same otherwise it will be considered as new request.