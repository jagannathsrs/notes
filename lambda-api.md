Different ways to interface with a GraphqlQL API deployed on AWS Lambda

AWS Lambda hard limits:
- **Function timeout** 900 seconds (15 minutes)
- **Invocation payload (request and response)**
  - 6 MB (synchronous)
  - 256 KB (asynchronous)

/ | Application Load Balancer | API Gateway | Lambda function URL | Lambda function URL with Cloudfront Distribution |
---- | ----- | ---- | ---- | ---- | 
Response payload limit | 1 mb | 10 mb | 6 mb | 6 mb |
Response time limit | 3600 secs | 29 secs | 15 mins | 180 s* |
Security | NA | NA | IAM,CORS | IAM,CORS |
