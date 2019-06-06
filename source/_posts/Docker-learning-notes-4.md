---
title: Docker learning notes(4)--SSL certificates
date: 2018-07-10 20:30:28
tags: [docker,SSL,Spring Boot] 
---
### Differences between Http and Https
● The HyperText Transfer Protocol (HTTP) protocol is used to transmit information between the Web browser and the web server. The HTTP protocol sends content in plaintext, and does not provide any way of data encryption. If an attacker intercepts the transmission message between the Web browser and the web server, the information can be read directly. Therefore, HTTP protocol is not suitable for transmitting some sensitive information, such as credit card number, password and other payment information.
<!-- more -->
● In order to solve this defect of the HTTP protocol, it is necessary to use another protocol: Hypertext Transfer Protocol Secure (HTTPS). In order to secure data transmission, HTTPS adds SSL protocol on the basis of HTTP. SSL relies on certificates to verify the identity of the server and encrypts the communication between the browser and the server.

![HTTPvsHTTPS](../../../../images/docker/HTTPvsHTTPS.jpg)

● The data transmitted by the HTTP protocol are not encrypted, that is, plaintext, so it is very unsafe to use the HTTP protocol to transmit privacy information. In order to ensure the encryption and transmission of these privacy data, the Netscape Co designed the SSL (Secure Sockets Layer) protocol to encrypt the data transmitted by the HTTP protocol--HTTPS. To put it simply, the HTTPS protocol is a network protocol constructed by SSL+HTTP protocol, which is capable of encrypting transmission and identity authentication,so it's more secure than HTTP protocol.

### Apply for a SSL certificate
● There are several ways to apply a SSL certificate, and I chose TrustAsia SSL certificate.The process of application will not be detailed.
Below are the certificates for Apache, IIS, Nginx and Tomcat:
![SSL-certificate-1](../../../../images/docker/SSL-certificate-1.png)

### Configure SSL for Spring Boot project
● Put the certificate( .jks file in tomcat folder) in the project at the application.yml level directory.
![SSL-certificate-2](../../../../images/docker/SSL-certificate-2.png)
● Add configurations for ssl in applcation.yml( or application.properties)
![SSL-configurations](../../../../images/docker/SSL-configurations.png)

● Add http redirect in the Spring Boot application file
```bash
@Bean
public EmbeddedServletContainerFactory servletContainer(){
    TomcatEmbeddedServletContainerFactory tomcat=new TomcatEmbeddedServletContainerFactory(){
        @Override
        protected void postProcessContext(Context context) {
            SecurityConstraint securityConstraint=new SecurityConstraint();
            securityConstraint.setUserConstraint("CONFIDENTIAL");//confidential
            SecurityCollection collection=new SecurityCollection();
            collection.addPattern("/*");
            securityConstraint.addCollection(collection);
            context.addConstraint(securityConstraint);
        }
    };
    tomcat.addAdditionalTomcatConnectors(httpConnector());
    return tomcat;
}

@Bean
public Connector httpConnector() {
    Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
    connector.setScheme("http");
    // The port number of the HTTP that Connector monitors
    connector.setPort(80);
    connector.setSecure(false);
    // Redirect port after monitoring HTTP port
    connector.setRedirectPort(443);
    return connector;
}
```

### Add 'Post Steps'-'Execute shell' in Jenkins
```bash
cd /var/lib/jenkins/jobs/wxblog
sudo docker build -t wxblog:$BUILD_NUMBER .
sudo docker ps -a | grep wxblog | awk '{print $1}' | xargs sudo docker rm -f
sudo docker run -d -p 80:80 -p 443:443 --name wxblog_$BUILD_NUMBER wxblog:$BUILD_NUMBER
sudo docker images|grep wxblog|awk '{print $3 }'|xargs sudo docker rmi
```
● DON'T FORGET to run both ports: <code>-p 80:80 -p 443:443</code>.  
<br><br>
● Congratulation! When you visit http://www.amazingxu.xyz , the website will redirect to https://www.amazingxu.xyz with a secure mark beside your address bar : )
