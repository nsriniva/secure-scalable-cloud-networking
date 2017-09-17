# resilient-scalable-cloud-networking
The Problem
Service(HTTP, SMTP, POP, IMAP) connections terminating at the server(physical/VM) providing the service
The compute surface is the attack surface
Non-optimal utilization of the cloud network due to the “impedance mismatch” between the high latency/low mtu WAN and low latency/high mtu cloud
Complicates Virtual Machine mobility

The Proposal
Terminate TCP connections and service protocol(HTTP, SMTP, POP, IMAP) sessions at the cloud edge
Carry the service layer request/response messages to/from the application servers asynchronously  via a message bus 
Collect Edge network, cloud message and application server load metrics and use them along with security/performance policies to secure and scale the cloud

Architecture
The solution involves the following components:
Edge WCCP router that transparently redirects incoming Web and Mail traffic to the TSAs
TSA(Transport/Session Adapter)s 
 terminate the TCP session
perform protocol(HTTP, SMTP, POP, IMAP) specific authentication 
pass request messages to Request Dispatchers  
receive response message(s) that are then streamed out via the appropriate TCP session.
Request Dispatcher(RD)s dispatch request messages to the appropriate Application Service Queue using a Message Broker

Message Broker(MB)s provide message publish/subscribe services to clients by receiving/forwarding messages from/to clients and/or other message brokers.
Instrumentation Agent(IA)s collects tcp, session, message, service queue and machine load metrics and pass them on to the Instrumentation Collector/Processor.
Instrumentation Collector/Processor(ICP) collates metrics from the various IAs and forwards appropriate processed metrics to the Security and Application Service Managers - the processing can be controlled with Instrumentation Processing scripts.

Security Manager (SM) uses metrics provided by the ICP along with Security Policy scripts to police application requests and sessions.
Application Service Manager(ASM) uses metrics provided by the ICP along with Application Service Policy scripts to manage Application Servers and the VMs that run them.
Application Server(AS)s read requests from the appropriate Application Service Queue, process them and write response messages to the appropriate TSA Queue for transmission to the client that made the request.

Advantages

The proposed solution builds on traditional TCP acceleration techniques[6] and simplifies the deployment of a "canonical cloud architecture” [1] by enhancing the network to provide “asynchronous, persistently queued events" and support "dynamically scalable CPUs".
Simplifies VM mobility
The TSAs provide a “World - Cloud barrier”(like the human body’s “Blood-Brain barrier”[8]) and improve security by separating out the compute surface.
Allows for easy detection of attacks like the “slow-motion DoS attack”[2] which would be difficult to detect without the application message visibility afforded by the proposed architecture.

References
[1]The Canonical Cloud Architecture - http://highscalability.com/blog/2009/8/7/t he-canonical-cloud-architecture.html

[2]New slow-motion DoS attack: just a few PCs, little fear of detection – http://arstechnica.com/business/2012/01/new-slow-motion-dos-attack-just-a-few-pcs-little-fear-of-detection/

[3] Qpid- https://cwiki.apache.org/qpid/index.html

[4] Nginx - http://nginx.org/

[5] OpenFabrics Enterprise Distribution - https://openfabrics.org/ofed-for-linux -ofed-for-windows/ofed-overview.html

[6] TCP Acceleration - http://en.wikipedia.org/wiki/TCP_acceleration

[7] Tengine - http://tengine.taobao.org/

[8] Blood-brain  barrier - http://en.wikipedia.org/wiki/Blood%E2%80%93brain_barrier
