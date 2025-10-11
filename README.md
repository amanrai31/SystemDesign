# SystemDesign

Resource => https://www.youtube.com/watch?v=s9Qh9fWeOAk&t=56s  |  https://algomaster.io/learn/system-design/what-is-system-design

AIM => Design Reliable, Scalable, Maintainable systems, highly available(minimizing downtime).

- Data-intensive applications => the quantity of data, the complexity of data, or the speed at which it is changing
- Compute-intensive => where CPU cycles are the bottleneck

 # Overview => 30 Most important system design concepts =>

- `Client-Server architecture` | `IP Address` | `DNS` | `Proxy/reverse Proxy` | `Latency`
- `HTTP/HTTPS` | `APIs` | `Rest API` | `GraphQL` | `gRPC` | `Websockets` | `WebHooks` | `WebRTC`
- `DB` | `SQL/noSQL` | `Vertical/Horizonal Scaling` | `Load Balancers` | `DB Indexing` 
- `Replication` | `Sharding - Horizontal/Vertical Partitioning` | `Caching` | `Denormalization` | `CAP Theorem`
- `Blob Storage` | `CDN` | `MicroServices` | `MessageQueues` | `RateLimiting` | `API Gateways` | `Idempotency`

1. Client-Server architecture => Client(website/App etc) sent request to server(waiting for requests), req made by client 1st goes to `DNS Server` for `IP resolution`, then the request is `proxied` and goes to `BFF`(Backend for frontend) Server(through load balancer maybe).

2. PROXY/ Forward PROXY => Server do not know who the client is. `Need` => 1. Hide client identity (IP masking) || 2. Control or filter outgoing traffic (e.g., company network restrictions) || 3. Cache responses for faster access || 4. Monitor usage. A VPN acts as a type of forward proxy.
   
3. Reverse-Proxy => Client does not know who the server is. A reverse proxy sits in front of one or more servers and acts on behalf of the servers. Clients send requests to the reverse proxy, which then forwards them to the appropriate backend server. `Need` => 1. Load balancing across multiple servers || 2. Caching responses to reduce load on backend || 3. SSL termination (handling HTTPS encryption centrally) || 4. Security / DDoS protection (hides real server IPs) || 5. Routing (e.g., different microservices or paths)


