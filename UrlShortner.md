## Url Shortner System Design

### Functional Requirements
1. **URL Shortening**:Given an original long URL, return a short URL.
2. **URL Redirection**: Short Url should redirect to original URL.
3. **Analytics**: User should be able to see noof clicks on the shorten URL.

### Non Functional Requirements
1. Scale: There can be around 1 trillion links to store.
2. Reads to Writes Ratio => 10:1, Reads>> Writes
3. Highly Available: System should be highly available otherwise user will not be able to access a link.
4. Concurrent clicks: System should be able to handle concurrent clicks count. Also popular URLs can have millions of clicks.

### Back of Envelope Estimation:
- Writes: 100 million writes per day
- Writes per second = 100 million /24*360 = 1160
- Reads per second = 10*1160 = 11600
- Average Url Length = 100 bytes
- Total storage = 1 Trillion * 100 bytes = 100 TB

### API Design
1. POST /api/v1/data/shorten
- Request Parameter {longUrl: URL string}
- return short URL

2. GET /api/v1/shortURL
- Returns long URL with http redirect response code 
- 301: permanent redirect, browser cache response
- 302: temporary redirect

3. GET /api/v1/count/shortURL
- Returns count analytics of how many times this url has been accessed or clicked.

### Database Design