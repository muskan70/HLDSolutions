## Rate Limiter

### Functional Requirements:
1. Rate limiter should be able to stop malicious users from using our system resources by sending excessive requests within a time frame.
2. System should be able configure API rate limit either at userId or Ip address.
3. Multiple rate limiting algorithms should be supported based on API requirements like token bucket, leaky bucket, sliding window…
4. User should be informed properly, show clear exceptions if request is throttled.

### Non Functional Requirements:
1. Highly Scalable: Rate limiter should handle high traffic of requests at a given time. 
2. Low Latency: Rate limiting should not slow down HTTP response time.
3. High fault tolerance: If rate limiter goes down, it should not impact the system.

