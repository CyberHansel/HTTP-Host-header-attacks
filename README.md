# **Web-app-testing-list**

## **HTTP Host header attacks ==================== **  
### Password reset poisoning
We visit https://example.com/security, browser will compose a request containing a Host header:
> GET /security HTTP/1.1  
> Host: example.com  

When a browser sends the request, URL will resolve to the IP address of server. When server receives the request, it refers to the Host header to determine the intended back-end and forwards the request accordingly to us.  

* password reset poisoning - changing Host: header to our malicious domain to obtain password reset link and steal the secret token. 

/forgot-password?temp-forgot-password-token=S0j4fkc7y9M9AgAPXu8kDsNG1pT9kPZ4
