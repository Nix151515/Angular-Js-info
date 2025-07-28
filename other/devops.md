Nginx is a popular open-source web server and reverse proxy known for its high performance and low resource utilization.

Definition: Piece of software on a server that handles HTTP requests.
Can be Web server (handling requests), Proxy server (caching, load balancing) as the only entry point to the systems

The purpose is configurable via 'nginx.conf' file, usually in '/etc/nginx' folder.
Things to set: port, server name, filesystem path to the files to display on static sites, cache duration, etc

Mostly used in Kubernetes containers, most as a specialized load balancer for incoming traffic (ingress controller).
The ingress controller is part of the cluster and not publicly available.
The one receiving the external traffic is the cloud load balanced (AWS, Google, etc) which reaches nginx controller.
What nginx was doing for web servers is also doing for k8 clusters.

Fireship -> Gateway between the internet and your backend infrastructure
Mostly on a linux server.
One or more servers can be defined on the conf file





Here's a more detailed breakdown:
Web Server:
Nginx can directly serve static content and handle incoming HTTP requests, similar to other web servers like Apache.

Reverse Proxy:
It can sit in front of other servers (like application servers) and forward client requests, potentially improving performance and security. 

Load Balancer:
Nginx can distribute traffic across multiple servers, preventing any single server from being overloaded. 

HTTP Cache:
Nginx can cache frequently accessed content, reducing the load on backend servers and speeding up response times. 

Mail Proxy:
Nginx supports proxying email protocols like IMAP, POP3, and SMTP. 

Open Source:
Nginx is free to use and distribute, with a large community contributing to its development and support. 

High Performance:
Nginx is known for its ability to handle a large number of concurrent connections with low resource consumption, making it suitable for high-traffic websites. 




A reverse proxy acts as an intermediary server, intercepting requests from clients (like web browsers) and forwarding them to one or more backend servers.
It sits in front of the servers, making it appear as a single point of access for clients while enhancing security, performance, and scalability

An API gateway acts as a single entry point for handling client requests to various backend services
API gateways are commonly used in microservices architectures to simplify client interactions and manage API traffic. 


                Docker = tool that help develop, ship, run apps in containers
It has everything packed to run the code everywhere
Key concepts: images and containers.
Image = the systems needed to run our code (recipe)
Containers = product of the image

Dockerfile = instructions to run the application (npm install, node app.js, the port)
docker build -> contruieste imaginea pe baza Dockerfile
docker run my_project -> ruleaza containerul generat pe baza imaginii (pot fi mai multe generate)
Containers can be debugged and observed for analytics.

We can have a giant container for the BFF, FE and DB, but don't do that.
Each should have their own containers, which are orchestrated by a compose.yaml file.
It uses the Dockerfile OR directly an image (for DB)
docker compose up -> porneste containerele


            Kubernetes

Kubernetes definitions


Pod = instanta de server. Mai multe similare, pt trafic mare

https://www.youtube.com/watch?v=JKxlsvZXG7c&t=14s&ab_channel=Fireship
