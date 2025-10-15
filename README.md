# SystemDesign

### Resource 

https://www.youtube.com/watch?v=s9Qh9fWeOAk&t=56s

https://algomaster.io/learn/system-design/what-is-system-design

https://github.com/ashishps1?tab=repositories

AIM => Design Reliable, Scalable, Maintainable systems, highly available(minimizing downtime).

- Data-intensive applications => the quantity of data, the complexity of data, or the speed at which it is changing
- Compute-intensive => where CPU cycles are the bottleneck

 # Overview => 30 Most important system design concepts =>

- `Client-Server architecture` | `IP Address` | `DNS` | `Proxy/reverse Proxy` | `Latency`
- `HTTP/HTTPS` | `APIs` | `Rest API` | `GraphQL` | `gRPC` | `Websockets` | `WebHooks` | `WebRTC`
- `DB` | `SQL/noSQL` | `Vertical/Horizontal Scaling` | `Load Balancers` | `DB Indexing` 
- `Replication` | `Sharding - Horizontal/Vertical Partitioning` | `Caching` | `Denormalization` | `CAP Theorem`
- `Blob Storage` | `CDN` | `MicroServices` | `MessageQueues` | `RateLimiting` | `API Gateways` | `Idempotency`

- Client-Server architecture => Client(website/App etc) sent request to server(waiting for requests), REQ made by client 1st `proxied`(if on pvt/corporate n/w) then goes to `DNS Server` for `IP resolution`, and goes CDN Server(1st proxy if publuc n/w)then forwarded to API Gateway(Zuul / Envoy / Kong), API Gateway redirects to respective `BFF`(Backend for frontend) Server(s). Each BE Server receives the request and calls multiple microservices & combine the response then sends to client via API Gateway. Each MS sits behind LB & each service may have its own DB & cache and they may call one or more other MS.

- PROXY/ Forward PROXY => Server do not know who the client is. `Need` => 1. Hide client identity (IP masking) || 2. Control or filter outgoing traffic (e.g., company network restrictions) || 3. Cache responses for faster access(can have its own DNS cache) || 4. Monitor usage. A VPN acts as a type of forward proxy.
   
- Reverse-Proxy => Client does not know who the server is. A reverse proxy sits in front of one or more servers(in front of each microservice) and acts on behalf of the servers. Clients send requests to the reverse proxy, which then forwards them to the appropriate backend server. `Need` => 1. Load balancing across multiple servers(maybe multiple servers of the same service) || 2. Caching responses to reduce load on backend || 3. SSL termination (handling HTTPS encryption centrally) || 4. Security / DDoS protection (hides real server IPs) || 5. Basic Routing (e.g., different microservices or paths) || 6. Rate limiting || 7. Monitoring & Logging || zero-trust networking(Each proxy validates internal tokens before forwarding traffic deeper.)

- API Gateway => Authenticates the user (JWT, OAuth) | Checks rate limits | Decides which backend to send to (routing) | SSL termination | Handles cross-cutting concerns (logging, tracing) | Might transform protocols (HTTP → gRPC) | Might handle caching. True IP masking happens only for backend services; clients still see the gateway’s IP. Some advanced setups use CDNs in front of the gateway for even more obfuscation and DDoS protection. Some microservices in our system use their own API Gateway(`Zero trust policy`- like a payment gateway) 

- BFF => handles business + frontend-specific stuff. Tailored backend layer for each frontend (mobile, web, TV). The BFF (Backend for Frontend) exists because each frontend (web, TV) needs different combinations and shapes of data.

**NOTE :** Every API Gateway is a Reverse Proxy, but not every Reverse Proxy is an API Gateway. The API Gateway and BFF handle north-south traffic. But reverse proxies in front of microservices handle east-west traffic.

**NOTE:** In large architectures, every service, whether CDN, API Gateway, BFF, Microservices, etc., sits behind a load balancer. LBs also work as proxy/reverse-proxy.

```css
Client (App/Web)
    │
    ▼
[CDN Edge Server](proxy)
    │
    ▼
[API Gateway](proxy) → [ BFF(optional) ] → [ Reverse Proxy / Load Balancer ] →  [ Microservice Cluster (many instances) ] → DB
```

- Request path =>  The request path depends on request type => 1. Static Content: Backend → CDN Cache → Browser | 2. Dynamic Content (cacheable - e.g., search results): Backend → CDN Cache (short TTL) → Browser | Dynamic Content (not cacheable - e.g., user profile): Backend → Browser directly (skips CDN) | Real-time Content (e.g., live chat, stock prices): Backend → WebSocket → Browser directly. `Response header control whether to cache or not and also the cache life`

- Response path example => You search for hotels in Airbnb => Response: JSON with 30 property listings : `Images: Sent to CDN for future requests` | `Search results JSON: Sent directly to browser (not cached)` | `Property thumbnails: Cached in CDN (reused across users)`. **Why not cache always through CDN*** - **User-specific data**: Your bookings are different from someone else's | **Real-time accuracy of bookings** | **Caching user profiles at edge servers is risky**

- HTTP/HTTPS => Client & server communicate through a set of rules, client sends a REQ which contains `headers` which has info about client like REQ type, browser type, cookies, REQ Body(in POST req) then the server sends RES which contains data or an error if something goes wrong. The RES is usually in JSON OR XML format. HTTP sends data as plain text(without encryption) but HTTPS encrypts data using SSL or TLS protocol. BUT `HTTP does not define :` 1. How req should be structured | 2. What format res should be in | 3. How diff clients should interact with server. Here we have APIs.

- API(Application programming interface) => Client sends a req to BFF server(API server) which has API gateway



------------


## 7 Techniques to protect API

- rate limiting, CORS, SQL & noSQL injection, firewall, VPN(on server side too), CSRF(cross site request forgery), XSS(cross site scripting)
