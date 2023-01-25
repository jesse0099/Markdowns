# AWS WAF (Web Application Firewall)

AWS WAF is a web application firewall service that lets us monitor web requests that are forwarded to an Amazon API Gateway API, an Amazon CloudFront distribution, or an Application Load Balancer. We can protect those resources based on conditions that we specify, such as the IP addresses that the requests originate from. [More...](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)

# How AWS WAF works

We use AWS WAF to control how our protected resources respond to HTTP(S) web requests. We do this by defining a web access control list (ACL) and then associating it with one or more web application resources that we want to protect. This allow us to respond to requests either with an HTTP 403 status code (Forbidden), or with a custom response,

# Benefits 

AWS WAF implementation supports hundreds of rules that can inspect any part of the web request with minimal latency impact to incoming traffic, also, a set of aws managed rules allow us to save time when getting started. Futhermore, there's no additional software to deploy, making it easy to deploy and mantain.

- Additional protection against web attacks using own criteria.
    - IP addresses that requests originate from
    - Country that requests originate from
    - Presence of SQL code that is likely to be malicious 
    - [More...](https://docs.aws.amazon.com/waf/latest/developerguide/waf-intro.html#:~:text=Additional%20protection%20against%20web%20attacks)
- Rules that we can reuse for multiple web applications
- Automated administration using the AWS WAF API
- Real-time metrics and sampled web request
- [More...](https://docs.aws.amazon.com/waf/latest/developerguide/waf-intro.html#:~:text=Using%20AWS%20WAF%20has%20several%20benefits%3A)

# Current Implementation

We're using the next set of WAF components:
    
- AWS WAF ACL

- AWS WAF Rule groups

- AWS WAF Managed Rules
