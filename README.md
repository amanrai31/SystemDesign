# SystemDesign

### Resource 

https://www.youtube.com/watch?v=s9Qh9fWeOAk&t=56s || https://www.youtube.com/watch?v=l3X1t3kpmwY

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

- `Client-Server architecture` => Client(website/App etc) sent request to server(waiting for requests), REQ made by client 1st `proxied`(if on pvt/corporate n/w) then goes to `DNS Server` for `IP resolution`, and goes CDN Server(1st proxy if publuc n/w)then forwarded to API Gateway(Zuul / Envoy / Kong), API Gateway redirects to respective `BFF`(Backend for frontend) Server(s). Each BE Server receives the request and calls multiple microservices & combine the response then sends to client via API Gateway. Each MS sits behind LB & each service may have its own DB & cache and they may call one or more other MS.

- `PROXY/ Forward PROXY` => Server do not know who the client is. `Need` => 1. Hide client identity (IP masking) || 2. Control or filter outgoing traffic (e.g., company network restrictions) || 3. Cache responses for faster access(can have its own DNS cache) || 4. Monitor usage. A VPN acts as a type of forward proxy.
   
- `Reverse-Proxy` => Client does not know who the server is. A reverse proxy sits in front of one or more servers(maybe in front of each microservice) and acts on behalf of the servers. Requests are intercepted by reverse proxy, which then forwards them to the appropriate backend server. `Need` => 1. Load balancing across multiple servers(maybe multiple servers of the same service) || 2. Caching responses to reduce load on backend || 3. SSL termination (handling HTTPS encryption centrally) || 4. Security / DDoS protection (hides real server IPs) || 5. Basic Routing (e.g., different microservices or paths) || 6. Rate limiting || 7. Monitoring & Logging || zero-trust networking(Each proxy validates internal tokens before forwarding traffic deeper.)

- `API Gateway` => Authenticates the user (JWT, OAuth) | Checks rate limits | Decides which backend to send to (routing) | SSL termination | Handles cross-cutting concerns (logging, tracing) | Might transform protocols (HTTP → gRPC) | Might handle caching. True IP masking happens only for backend services; clients still see the gateway’s IP. Some advanced setups use CDNs in front of the gateway for even more obfuscation and DDoS protection. Some microservices in our system use their own API Gateway(`Zero trust policy`- like a payment gateway) 

- `BFF` => handles business + frontend-specific stuff. Tailored backend layer for each frontend (mobile, web, TV). The BFF (Backend for Frontend) exists because each frontend (web, TV) needs different combinations and shapes of data.

**NOTE :** Every API Gateway is a Reverse Proxy, but not every Reverse Proxy is an API Gateway. The API Gateway and BFF handle north-south traffic. But reverse proxies in front of microservices handle east-west traffic.

```css
Client (App/Web)
    │
    ▼
[CDN Edge Server](proxy)
    │
    ▼
[API Gateway](proxy) → [ BFF(optional) ] → [ Reverse Proxy / Load Balancer ] →  [ Microservice Cluster (many instances) ] → DB
```

- `Request path` =>  The request path depends on request type => 1. Static Content: Backend → CDN Cache → Browser || 2. Dynamic Content (cacheable - e.g., search results): Backend → CDN Cache (short TTL) → Browser || 3. Dynamic Content (not cacheable - e.g., user profile): Backend → Browser directly (skips CDN) || 4. Real-time Content (e.g., live chat, stock prices): Backend → WebSocket → Browser directly. `Response header controls whether to cache or not and also the cache life`

- `Response path example` => You search for hotels in Airbnb => Response: JSON with 30 property listings: `Images: Sent to CDN for future requests` | `Search results JSON: Sent directly to browser (not cached)` | `Property thumbnails: Cached in CDN (reused across users)`. **Why not cache always through CDN*** - **User-specific data**: Your bookings are different from someone else's | **Real-time accuracy of bookings** | **Caching user profiles at edge servers is risky**

- `HTTP/HTTPS` => Client & server communicate through a set of rules, client sends a REQ which contains `headers` which has info about client like REQ type, browser type, cookies, REQ `Body`(in POST req) then the server sends RES which contains data or an error if something goes wrong. The RES is usually in JSON OR XML format. HTTP sends data as plain text(without encryption) but HTTPS encrypts data using SSL or TLS protocol. BUT `HTTP does not define :` 1. How req should be structured | 2. What format res should be in | 3. How diff clients should interact with server. Here we have APIs.

- `API(Application programming interface)` => API let client communicate with server. APIs are hosted on a server(apiGateway, BFF or other services) process the req and interact with DB or other services & prepare a response & send back to client in JSON or XML format. 2 most comman API type are => `rest` & `graphQL`. Some other popular API types - `gRPC`, `Websockets`, `WebHooks`, `WebRTC`

- `DBs` => Servers that are used to store & retrieve data. `SQL` - consistency, relational | `noSQL` - scalable, high-availability, flexible schema

- `Vertical & horizontal scaling of Application Servers` => `VS` - Scaling a existing server by adding more CPU, RAM, Storage (Expensive & single point of failure) | `HS` => Distributes the workload among multiple servers (We need load balancer here)

- `LoadBalancer` => In large architectures, every service, whether CDN, API Gateway, BFF, Microservices, etc., sits behind a load balancer. LBs also work as proxy/reverse-proxy. Uses LB algorithms like Round-Robin, least connections, IP Hashing

**NOTE :** Now we have scaled application server but as traffic grows, the volume of data also increases. We have to index DB & scale DB - `DB scaling`

- `Indexing of DB` => It works the same way as index of a book does. It speeds up the `read queries`. Helps DB to quickly locate the required data without scanning the whole table. Stores the `column value along with pointer` to actual data rows in the table. Indexing speeds read but slow down write(Since index need to be updated whenever data changes) that's why Index created on frequently queried columns.

- What will happen when read request grows huge - `Replication` => Make multiple read replicas & 1 or more write replicas (primary replica). When data is witten to primary DB (Write replica), it gets copied to all read replicas to sync them. It solves the read traffic & ensures high-availability as if write replica goes down the we can make one of the read replicas as write replica.

- Scaling write operations - `Sharding` => Instead of storing all data on single server we split data on smaller servers(Shards) & distribute them accross multiple servers. We use sharding key. `Horizontal partitioning` - Slit data by rows | `Vertical partitioning` - Split DB by column.

- `Caching` => Optimizing performance by storing frequently accessed data in `memory` (not disk). Our application looks up the cache 1st, if data is there it returns quickly and if data is not cached then it queries the DB. To make cache relevent with time we use TTL(Time to live value)

- `Denormalization` => It reduces number of joins by combining related data into a single table even if some data gets duplicated. Denormalization is often used in read-heavy applications. But it leads to increase storage

In distributed systems where we have multiple servers, DBs over multiple data centers. 

- `CAP Theorem` => No distributed system can achive `Consistency`, `Availability` & `Partion tolerance` all at the same time

- `Blob Storage` => Traditional DBs are not designed to store large, unstructured files efficiently. Files like images, videos, pdfs are stored inside logical containers/buckets. in blob storage like amazon s3, each 
file gets a unique URL.

- Syncronus comm(waiting for immediate res) do not always scale well, `message queue` enables the services to comm `asynchronously` that allows req to process without blocking other operations. MQ de-couple services & improve scalability. Producer put data in MQ & consumer comsumes it asynchronously.

- `Rate-limiting` => We assign a quota to particular IP & put a cap like 100req/sec. Rate-limiting algo - `Fixed window`, `sliding window`, `Token bucket`

- `Idempotency` => Ensures that repeated requests produce the same result, Each req is assigned a unique ID before processing the system check if req is already handled if yes then it ignores the duplicate req.

------------


## 7 Techniques to protect API

- rate limiting, CORS, SQL & noSQL injection, firewall, VPN(on server side too), CSRF(cross site request forgery), XSS(cross site scripting)
