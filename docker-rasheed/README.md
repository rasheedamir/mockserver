**Step 1: Build Docker Image**

`sudo docker build -t mockeserver_img .`

**Step 2: Run Docker Container**

`sudo docker run -d -v /home/rasheed/Projects/trackingtool-web:/trakly -p 9000:9000 -p 3001:3001 -p 4022:22 -t trakly_img`

`docker run -d -p 1080:1080 -p 1090:1090 -p 4022:22 -t mockserver_img`

`/opt/mockserver/run_mockserver.sh -logLevel INFO -serverPort 1080 -proxyPort 1090 -proxyRemotePort 80 -proxyRemoteHost https://wamsdubclus001rest-hs.cloudapp.net/api/`

HTTPS

run_mockserver.sh -logLevel INFO -serverPort 1080 -proxyPort 1090 -proxyRemotePort 443 -proxyRemoteHost https://wamsdubclus001rest-hs.cloudapp.net/

_**NOTE**_ provide valid source code location i.e. _/home/rasheed/Projects/trackingtool-web_

**Step 3: SSH into the Container**

`ssh -p 4022 aurora@localhost`
when prompted provide password: `aurora`

**Step 4: Change Directory in Container**

`cd ../../aurora/`

Unable to connect to socket localhost/127.0.0.1:1090

copy the CertificateAuthorityCertificate.pem from mockserver-core and then import:
keytool -import -alias MockServerRoot -keystore truststore.jks -file CertificateAuthorityCertificate.pem 

sudo ./run_mockserver.sh -logLevel INFO -serverPort 1080 -proxyPort 1090 -proxyRemotePort 443 -proxyRemoteHost wamsdubclus001rest-hs.cloudapp.net

sudo find -name mockserver_request.log

#

url = "https://0.0.0.0:1090/api/Assets";
//url = "https://wamsdubclus001rest-hs.cloudapp.net:443/api/Assets";

# in the end after the HTTP call has been made...
new ProxyClient("localhost", 1090)
        .dumpToLogAsJava(
                request()
                        .withMethod("POST")
                        .withPath("/api/Assets")
        );

        // TODO: REMOVE IT
        url = "https://0.0.0.0:1090/api/Assets";

        // TODO: REMOVE IT
        new ProxyClient("localhost", 1090)
                .dumpToLogAsJava(
                        request()
                                .withMethod("POST")
                                .withPath("/api/Assets")
                )
                .dumpToLogAsJSON(
                        request()
                                .withMethod("POST")
                                .withPath("/api/Assets")
                );

        //getServiceSender().getFromApplication(buConfig, ApplicationName.EMPApp)


