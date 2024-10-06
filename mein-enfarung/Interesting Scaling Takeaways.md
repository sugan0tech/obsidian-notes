# JioCinema IPL Live Streaming: Scaling Strategy

## Auditing

### Frontend
- Any features should go via **feature flag** (enables easy rollback during game time).
- **Classification of features** by priority (P1, P2) with appropriate responses based on priority level.

### Backend & Infrastructure
- Typical request flow: 
  - `app -> CDN -> load balancer -> origin servers -> database`
- **Database scaling** poses challenges due to its complexity.

--- 
## Key Takeaways

### Multi-CDN Architecture
- Limitations on individual CDN performance.
- Utilization of **Multi-CDN optimizer**, which acts like a load balancer for CDNs.

### Static Response Snapshots
- Take snapshots of API responses before matches. In case of failure, CDN can serve these **static API response snapshots** instead of querying the origin servers.

### Network Bandwidth Usage
- JioCinema consumed **75% of India’s cloud providers' internet bandwidth** at peak times.

### Contingency Planning
- Always have a **Plan B** ready to ensure service continuity.

### Database Scaling
- **Caching** is the optimal solution to mitigate sudden database load spikes.
- Focus on **degradation (scaling down)**, readiness probes, and liveliness checks.

### Cache Offloading
- Achieving **90% cache offload** is ideal and is considered a significant milestone (referred to as "cinema movement").

## Additional Insights

- **Feature flagging** managed via config services, allowing dynamic enabling/disabling based on geography.
- **Charles testing tool** for traffic monitoring and debugging.
- Implement **exponential backoff** for API retries.
- Request flow: 
  - `client -> Multi-CDN -> load balancer -> origin server -> database`
- **DB autoscaling** is not recommended due to long scale-up times (e.g., 45 minutes to scale 5 instances). **Prescaling** based on traffic calculations is preferred.
- During **hockey-stick traffic spikes**, increasing cache TTL may or may not help. Serving **static API responses** during these spikes is an alternative.
- Always be prepared with **Plan B, C, and D**.
- **Multi-CDN optimizer** decides where to route traffic, critical for live streaming.
- **Cache offloading < 90%** requires optimization.
- **Cache policy design** for CDN is crucial, especially in offloading and monitoring.
- Use of **Kafka** for messaging.
- Mitigating **DNS issues** at the backend.

## References
- [How JioCinema live streams IPL to 20 million concurrent devices w/ Prachi Sharma](https://www.youtube.com/watch?v=36N1Bz7qW0A)
- [Behind the Stream: JioCinema’s Workflow for Major Events](https://blog.jiocinema.com/behind-the-stream-jiocinemas-workflow-for-major-events/ff)

--- 