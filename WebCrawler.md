## Web Crawler System Design

### Functional Requirements
1. Given some seed URLs, extract content and other url links from them and further crawl them.
2. System should be able to prioritize Urls.
3. It should follow crawling policies per host i.e Robots.txt
4. Avoid Duplication : About 30% of content on web is duplicate, avoid storing duplicate content.

### Non Functional Riquirements
1. **Scalability**: System should be able to crawl billions of pages per month, It should be able to crawl web pages in parellelism
2. **Robustness**: System should handle bad HTML, unresponsive servers, crashes, malicious links etc all the edge cases.
3. **Extensible**: System should be extendible to crawl different types of content likes images, videos, PDFs etc.

### Back Of Envelope Estimation
1. No.of web pages to crawl per month = 1 billion
2. QPS = 1 billion/ 30 days /24 hours / 3600 secons = 400 pages per second
2. Average page size = 500 KB
3. Storage required = 1 billion * 500 KB = 500 TB

### Database schema
1. URL data Table
- URL link: string
- status: enum(done,pending)
- last Crawled : Date
- content Type: enum(HTML, image, pdf)
- content size: in KB
- URL host/ domain : string

2. Content Table
- Url link: string
- title: string
- keywords: []string
- text content: string

3. Links Table
- source URL: string
- Target URL : string
- Anchor text: string
- Link Type: internal or external

### Design Components
1. URL Frontier
2. DNS Resolver+ Cachier
3. Content Downloader
4. Content Parser <-> Content Storage
4. Link Extractor
5. Url Filter <-> Url Storage
![Web Crawler](./images/WebCrawler.png)

### URL Frontier
**Priortizer-> Front Queues -> Front Queue Selector -> Back Queues Router -> Back Queues -> Back Queues Selector -> Workers** 
![URL Frontier](./images/UrlFrontier.png)

### System GoThrough







