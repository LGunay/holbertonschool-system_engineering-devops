## Simple web stack

The diagram you've provided represents a basic web application architecture. Let's break down each component and its role as you've requested:

1.**Server Location in Data Centers**: Servers are generally located in data centers, which are facilities with the necessary infrastructure to house computer systems and associated components, such as telecommunications and storage systems. Data centers provide high-security, controlled climates, and redundant power supplies to keep servers operational 24/7.

2.**Physical and Virtual Servers**: A server can be a physical machine, a dedicated computer built for processing requests and delivering data to another computer over the internet or a local network. Alternatively, a server can be virtual – a software-based server running on a physical server, which is known as virtualization. This allows for more efficient resource utilization by running multiple virtual servers on a single physical server.

3.**Server Operating Systems (OS)**: A server runs an operating system, which is software that manages the server's hardware and software resources and provides common services for computer programs. The OS allows the server software to perform its duties (e.g., web server, application server) and can vary from Windows Server to various distributions of Linux or Unix.

4.**Web Server Role**: The web server's role, as indicated by Nginx in the diagram, is to serve static content, like HTML pages, CSS, and JavaScript files. When a web server receives an HTTP request, it responds with the requested pages, which are then rendered in the client's web browser.

5.**Application Server Role**: The application server's role is to generate dynamic content. It runs the application's back-end logic, processes user requests, performs calculations, and makes decisions. The results are often dynamic, changing based on user interaction and other factors.

6.**Database Role**: The database's role, represented by MySQL in the diagram, is to store, retrieve, and manage the application's data persistently. It handles queries from the application server and safely stores the data so that it can be consistently retrieved and updated.

7.**DNS Role**: The DNS (Domain Name System) translates human-readable domain names (like www.foobar.com) into machine-readable IP addresses (like 8.8.8.8). It's essentially the phonebook of the internet, allowing users to access websites using familiar domain names instead of numerical IP addresses.

8.**A Record**: In DNS terminology, an 'A' record maps a domain to the physical IP address of a computer hosting that domain's services. In the diagram, www.foobar.com is an A record because it resolves to the IP address 8.8.8.8

9.**Single Point of Failure**: The server in the diagram is a single point of failure because it does not have any redundant components. If any part of the server fails (hardware, network, power supply), the entire service for www.foobar.com could become unavailable.

10.**Downtime During Deployment**: If new code is deployed and the web server needs to be restarted, the website could be temporarily down. This is because there are no redundant servers or a load balancer to handle requests during the downtime.

11.**Lack of Scalability**: The infrastructure in the diagram cannot scale because it is a single server setup. If the traffic exceeds the server's capacity, the system will not be able to handle the load, leading to potential outages or degraded performance.

12.**Network Communication**: The server communicates over a network using the TCP/IP protocol, which is the fundamental suite of protocols used for data transmission over the internet. TCP/IP handles the routing and delivery of data packets from the client to the server and back.

In summary, the diagram

shows a basic web application architecture with a single server that may be located in a data center. This server could be a physical machine or a virtualized environment and runs an operating system that supports the web server software like Nginx, the application server, and the database server. The web server serves static content, the application server computes dynamic content, and the database stores application data. The DNS server translates domain names into IP addresses, and in this case, www.foobar.com has an A record pointing to an IP address. The setup is a single point of failure, meaning any part of it failing would cause the entire service to go down. Also, since there are no redundant systems or scalability solutions in place, the service cannot handle traffic beyond its maximum capacity and would experience downtime during maintenance or code deployment. All communication occurs over a network using the TCP/IP protocol suite.

## Distributed web infrastructure

Based on the diagram provided, it shows a load balancing setup using HAProxy, which distributes incoming client requests (load) across multiple servers. The key points in the diagram indicate the use of a Round-Robin algorithm for load distribution, and both server setups labeled as "Active."

In an Active-Active setup, all servers are handling requests simultaneously. The Round-Robin algorithm suggests that each server takes turns receiving new incoming connections. This points to an Active-Active configuration, as the load balancer is distributing the load across both servers, and both are in an active state, ready to handle requests.

In contrast, an Active-Passive setup would have one server handling all the requests while the other server is on standby, ready to take over if the active server fails or becomes unavailable. The diagram does not indicate any server being in a passive or standby mode.

Given that both servers are labeled "Active" and there's a Round-Robin algorithm in use, it is reasonable to conclude that the load balancer is configured for an Active-Active setup. This setup is generally used to maximize resource utilization and ensure high availability without any idle hardware.

## Secured and monitored web infrastructure

Based on the information from the diagram and your questions, here's an explanation for each:

1. **Firewalls**: Firewalls are network security devices that monitor and control incoming and outgoing network traffic based on predetermined security rules. Their primary purpose is to establish a barrier between your internal network and incoming traffic from external sources (such as the internet) to block malicious traffic like viruses and hackers.

2. **Traffic Served Over HTTPS**: HTTPS (Hypertext Transfer Protocol Secure) is an extension of HTTP. It is used for secure communication over a computer network and is widely used on the internet. In HTTPS, the communication protocol is encrypted using Transport Layer Security (TLS), or formerly, its predecessor, Secure Sockets Layer (SSL). The reason for using HTTPS is to ensure that the data exchanged between the client and server is encrypted and secure, which is crucial for protecting user privacy and securing transactions.

3. **Monitoring**: Monitoring tools are used to continuously observe systems and services to check their health, performance, and reliability. This helps in detecting any issues early on, so they can be addressed before they become serious problems. It can also provide insights into trends and patterns that can help with capacity planning and optimization.

4. **Monitoring Tool Data Collection**: Monitoring tools like SumoLogic can collect data in various ways, including:

* **Agent-based collection**: Installing agents on the server that collect logs, metrics, and performance data, then send them to the monitoring tool.
* **Agentless collection**: Using existing protocols and services (like SNMP, WMI, syslog, etc.) to remotely collect monitoring data.
* **API integration**: Connecting with APIs provided by the services or platforms to retrieve metrics and logs.
* **Log collection**: Aggregating log files from servers and applications, which can be analyzed for insights.
5. **Monitoring Web Server QPS (Queries Per Second)**: To monitor the QPS of a web server:

* **Configure the web server logging**: Ensure that your web server is configured to log the necessary request data in its access logs. This includes the timestamp of each request, which is essential for calculating QPS.
* **Set up a monitoring tool**: Use a tool like SumoLogic, which can ingest access logs from the web server. Set up the tool to read the incoming log data.
* **Create metrics**: Within the monitoring tool, create a metric or query to count the number of requests (queries) per second. This is typically done by counting the number of lines (representing requests) in the access log over time and dividing by the number of seconds in that timeframe.
* **Set up a dashboard**: Use the monitoring tool to set up a dashboard that will display the QPS in real-time. You may also want to set alerts for when the QPS goes above or below certain thresholds, which could indicate performance issues or downtime.

## Scale up


The image you've uploaded appears to depict a high-availability architecture that uses multiple HAProxy instances configured in what seems to be a cluster setup.

In this configuration, there are two layers of HAProxy instances:

1. **Master HAProxy**: This is likely the primary entry point for incoming HTTPS traffic. It uses the Round-Robin algorithm to distribute the traffic across the second layer of load balancers, the HAProxy cluster.

2. **Cluster HAProxy**: This layer consists of multiple HAProxy instances that receive distributed traffic from the Master HAProxy. These instances then further distribute the traffic to a pool of web servers.

The purpose of having a clustered setup for the HAProxy instances is to ensure high availability. This means that if one of the HAProxy instances in the cluster fails, the others can continue to distribute the load without interruption. This setup is designed to remove single points of failure, which is crucial for maintaining service uptime and reliability.

Each web server is connected to an application server with its own database, and all of these are monitored by SumoLogic, which is a cloud-based log management and analytics service. This setup allows for the scaling of traffic handling and resource management across the application's infrastructure.

In summary, the diagram shows a robust and resilient web application architecture designed to handle significant amounts of traffic with redundancy at the load balancing layer to prevent downtime in case of individual component failures.

Authors: Javid Jalilov, Gunay Gasimova
