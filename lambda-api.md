Different ways to interface with a GraphqlQL API deployed on AWS Lambda

AWS Lambda hard limits:
- **Function timeout** 900 seconds (15 minutes)
- **Invocation payload (request and response)**
  - 6 MB (synchronous)
  - 256 KB (asynchronous)

/ | Application Load Balancer | API Gateway | Lambda function URL | Lambda function URL with Cloudfront Distribution |
---- | ----- | ---- | ---- | ---- | 
Request payload limit | 0 | 10 mb | 0 | 0 |
Response payload limit | 0 | 10 mb | 0 | 0 |
Response time limit | 3600 secs | 29 secs | 0 | 0 |
Security | NA | NA | IAM,CORS | IAM,CORS |
Additional functionalities | NA | NA | NA | NA |
