# Web-app-testing-list

## HTTP Host header attacks =======================================  
We visit https://example.com/security, browser will compose a request containing a Host header:
> GET /security HTTP/1.1  
> Host: example.com  

The purpose of the HTTP Host header is to help identify which back-end component the client wants to communicate with and route request to right application.  
* Possible scenario is when a single web server hosts multiple websites or applications.
* When websites are hosted on distinct back-end servers and all traffic between the client and servers is routed through an intermediary system like load balancer or  reverse proxy server. This setup is especially prevalent in cases where clients access the website via a content delivery network (CDN).
* 

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
sd


