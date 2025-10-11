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

- Client-Server architecture => Client(website/App etc) sent request to server(waiting for requests), REQ made by client 1st goes to `DNS Server` for `IP resolution`, then the request is `proxied` and goes to API Gateway(Zuul / Envoy / Kong) then to `BFF`(Backend for frontend) Server. ReverseProxy server OR BFF server routes the request to the desired microservice through API Gateway

- PROXY/ Forward PROXY => Server do not know who the client is. `Need` => 1. Hide client identity (IP masking) || 2. Control or filter outgoing traffic (e.g., company network restrictions) || 3. Cache responses for faster access || 4. Monitor usage. A VPN acts as a type of forward proxy.
   
- Reverse-Proxy => Client does not know who the server is. A reverse proxy sits in front of one or more servers(in front of each microservice) and acts on behalf of the servers. Clients send requests to the reverse proxy, which then forwards them to the appropriate backend server. `Need` => 1. Load balancing across multiple servers(maybe multiple servers of same service) || 2. Caching responses to reduce load on backend || 3. SSL termination (handling HTTPS encryption centrally) || 4. Security / DDoS protection (hides real server IPs) || 5. Basic Routing (e.g., different microservices or paths) || 6. Rate limiting || 7. Monitoring & Logging || zero-trust networking(Each proxy validates internal tokens before forwarding traffic deeper.)

- API Gateway => 

**NOTE :** Every API Gateway is a Reverse Proxy, but not every Reverse Proxy is an API Gateway. The API Gateway and BFF handle north-south traffic. But reverse proxies in front of microservices handle east-west traffic

- HTTP/HTTPS => Client & server communicate through a set of rules, client sends a REQ which contains `headers` which has info about client like REQ type, browser type, cookies, REQ Body(in POST req) then the server sends RES which contains data or an error if something goes wrong. The RES is usually in JSON OR XML format. HTTP sends data as plain text(without encryption) but HTTPS encrypts data using SSL or TLS protocol. BUT `HTTP does not define :` 1. How req should be structured | 2. What format res should be in | 3. How diff clients should interact with server. Here we have APIs.

- API(Application programming interface) => Client sends a req to BFF server(API server) which has API gateway

```css
Client → [ API Gateway ] → [ BFF ] → [ Reverse Proxy / Load Balancer ] →  [ Microservice Cluster (many instances) ]

```

