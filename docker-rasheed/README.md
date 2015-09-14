RUN MOCK SERVER

**Step 1: Build Docker Image**

`sudo docker build -t mockserver_img .`

**Step 2: Run Docker Container**

`docker run -d -p 1080:1080 -p 1090:1090 -p 4022:22 -t mockserver_img`

**Step 3: SSH into the Container**

`ssh -p 4022 aurora@localhost`
when prompted provide password: `aurora`

**Step 4: Change Directory in Container**

`cd ../../opt/mockserver`

**Step 5: Run the mockserver**

`sudo ./run_mockserver.sh -logLevel INFO -serverPort 1080 -proxyPort 1090 -proxyRemotePort 443 -proxyRemoteHost wamsdubclus001rest-hs.cloudapp.net`

That should run the mockserver!

MAKE CHANGES IN YOUR CODE

1. Add Certificate
copy the CertificateAuthorityCertificate.pem from mockserver-core and then import:

keytool -import -alias MockServerRoot -keystore truststore.jks -file CertificateAuthorityCertificate.pem 

2. Add dependency in the POM file

        <!-- mockserver dependencies -->
        <dependency>
            <groupId>org.mock-server</groupId>
            <artifactId>mockserver-netty</artifactId>
            <version>3.9.17</version>
        </dependency>

3. Make changes in your code to proxy all requests through

`https://0.0.0.0:1090/`

host: 0.0.0.0
port: 1090

protocol: based on the one where you are proxing or forwarding...

remember the default port of https is 443

so, the actual call: 

url = "https://wamsdubclus001rest-hs.cloudapp.net:443/api/Assets";

will be proxied through:

url = "https://0.0.0.0:1090/api/Assets";

so, that it can log all requests and responses!

4. Add following to log requests and responses after each REST call:

        new ProxyClient("localhost", 1090)
                .dumpToLogAsJava(
                        request()
                )
                .dumpToLogAsJSON(
                        request()
                );

5. To copy the produced request/response log file (mockserver_request.log) can be copied like this:

sudo find -name mockserver_request.log

docker cp 591d6358fb6d:/opt/mockserver/mockserver_request.log /home/rasheed/Projects/mockserver/docker-rasheed

i.e.
docker cp <containerId>:/file/path/within/container /host/path/target