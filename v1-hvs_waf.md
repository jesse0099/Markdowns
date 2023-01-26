# AWS WAF (Web Application Firewall)

AWS WAF is a web application firewall service that lets us monitor web requests forwarded to an Amazon API Gateway API, an Amazon CloudFront distribution, or an Application Load Balancer. We can protect those resources based on conditions we specify, such as the IP addresses where the requests originate. [More...](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html)

# How AWS WAF works

We use AWS WAF to control how our protected resources respond to HTTP(S) web requests. We do this by defining a web access control list (ACL) and associating it with one or more web application resources we want to protect. This allows us to respond to requests either with an HTTP 403 status code (Forbidden) or with a custom response.

# Benefits 

AWS WAF implementation supports hundreds of rules that can inspect any part of the web request with minimal latency impact on incoming traffic; also, a set of AWS-managed rules allow us to save time when getting started. Furthermore, there's no additional software to deploy, making it easy to deploy and maintain.

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
    
- AWS WAF ACLs
    - ***v1-hvs-waff*** 
>
- AWS WAF Rule groups
    - ***v1-hvs-ip-whitelist-rule***
>
- AWS WAF Managed Rules
    - ***AWS-AWSManagedRulesBotControlRuleSet***
    - ***AWS-AWSManagedRulesAmazonIpReputationList***
>
- Use-case-specific rule groups
    - ***AWS-AWSManagedRulesPHPRuleSet***
>
- Custom Rules
    - ***Geolocation***

# Web ACLs

It gives us fine-grained control over all HTTP(S) web requests that our protected resource responds to.

***v1-hvs-waff:***

# Rule groups

Rule groups are reusable sets of rules we can add to a web ACL.

***v1-hvs-ip-whitelist-rule:***

It's composed of rules (IP sets) that allow each team access to v1-hvs server. The only exception is **percy-aws-resources-ip-list** which is intended to be used as a list of allowed AWS resources IPs.

| Rule name                 | Action     | Current IPs  |
|---                        |---         |---           |
|*bamboo-team-ip-list* |Allow |186.15.131.65/32 |
|   |   |181.194.226.200/32 |
|   |   |190.113.110.47/32 |          
|   |   |   |
|*sharmams-team-ip-list* |Allow |179.27.68.154/32 |
|   |   |   |
|*elliman-team-ip-list* |Allow |92.84.52.66/32 |
|   |   |182.74.119.238/32|
|   |   |   |
|*percy-aws-resources-ip-list* |Allow |13.58.172.165/32 |


# Managed Rules

It protects against common application vulnerabilities or other unwanted traffic without having to write our own rules.

The below-managed rules make use of **AWS WAF intelligent threat mitigation**. [More...](https://docs.aws.amazon.com/waf/latest/developerguide/waf-managed-protections.html#:~:text=AWS%20WAF%20intelligent%20threat%20mitigation)

***BotControlRuleSet:*** The Bot Control managed rule group provides rules to block and manage requests from bots. Bots can consume excess resources, skew business metrics, cause downtime, and perform malicious activities. 

***AmazonIpReputationList:*** It contains rules based on Amazon's internal threat intelligence. This is useful to block IP addresses typically associated with bots or other threats.

# Use-case-specific rule groups

***PHPRuleSet:*** The PHP application rule group contains rules that block request patterns associated with exploiting vulnerabilities specific to the use of the PHP programming language, including the injection of unsafe PHP functions. This can help prevent the exploitation of vulnerabilities that permit attackers to remotely run code or commands for which they are not authorized.

# Custom Rules

AWS allows us to write custom rules, providing a set of basic rule units to define our firewall logic no matter how complex it could be. For example, we could want to avoid traffic from certain countries. With AWS geolocation matches, It's an easy task.

***Geolocation:*** It blocks requests from countries different from Canada and the United States.

Learn more on: [Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/what-is-aws-waf.html)