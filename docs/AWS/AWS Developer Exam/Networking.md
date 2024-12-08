# Networking

## CloudFront

Amazon S3 static websites use the HTTP protocol only and you cannot enable HTTPS. To enable HTTPS connections to your S3 static website, use an Amazon **CloudFront distribution configured with an SSL/TLS certificate**. This will ensure that connections between clients and the CloudFront distribution are encrypted in-transit.

## API Gateway

Data trasnformation: API Gateway does not support SOAP APIs. An API's method request can take a payload in a different format from the corresponding integration request payload, as required in the backend. Similarly, the backend may return an integration response payload different from the method response payload, as expected by the frontend. API Gateway lets you use **mapping templates** to map the payload from a method request to the corresponding integration request and from an integration response to the corresponding method response.