# Web-app-testing-list

## HTTP Host header attacks =======================================  
We visit https://example.com/security, browser will compose a request containing a Host header:
> GET /security HTTP/1.1  
> Host: example.com  

The purpose of the HTTP Host header is to help identify which back-end component the client wants to communicate with and route request to right application.  
* Possible scenario is when a single web server hosts multiple websites or applications.
* When websites are hosted on distinct back-end servers and all traffic between the client and servers is routed through an intermediary system like load balancer or  reverse proxy server (they need to know on which back-end server to route requests). This setup is especially prevalent in cases where clients access the website via a content delivery network (CDN).

Problem is when server implicitly trusts the Host: header, and fails to validate or escape it properly  

### Password reset poisoning
Changing header Host: from legit to our malicious domain to obtain password reset link which include the unique reset token.   
1.) In Burp "POST /forgot-password" request body change the username parameter and change the Host: header to your exploit server's domain name  
2.) In malicious servers access.log obtain reset token  
3.) In Burp use our acc genuine password reset link from email, but change token parametre in URL.  
>/forgot-password?temp-forgot-password-token=S0j4fkc7y9M9AgAPXu8kDsNG1pT9kPZ4    

### Host header authentication bypass - change "Host: localhost"
If we can change Host: header to any value and still successfully access the home page.  
1.) robots.txt includes an /admin.  
2.) We cant access /admin, theres error message "Admin interface only available to local users"  
3.) In Burp change Host: header to localhost  
> Host: localhost  

### Web cache poisoning via ambiguous requests
To reduce latency cache sits between the back-end server and the user where it saves (caches) the responses to particular requests, usually for a fixed amount of time. If another user sends equivalent request, cache simply serves a copy of the cached response w/o interfering with back-end server. Caches identify  requests by comparing "cache key" (subset of request components). 
* Typically: request line and Host header. Components of the request that are not included in the cache key are said to be "unkeyed".  
* If the cache key of an incoming request matches the key of a previous request cache server will serve a copy of the cached response 
* Other components of the request are ignored!  

Any web cache poisoning attack relies on manipulation of unkeyed inputs, such as headers  

1.) Identifying unkeyed inputs that are supported by the server - Burp Comparer to compare the response with and without the injected input or Param Miner Burp extension (When test for unkeyed inputs on a live website, there is a risk of inadvertently causing the cache to serve your generated responses to real users. Therefore, it is important to make sure that your requests all have a unique cache key so that they will only be served to you.)




































































