# Web-app-testing-list

## HTTP Host header attacks =======================================  
We visit https://example.com/security, browser will compose a request containing a Host header:
> GET /security HTTP/1.1  
> Host: example.com  

When a browser sends the request, URL will resolve to the IP address of server. When server receives the request, it refers to the Host header to determine the intended back-end and forwards the request accordingly to us.   

### Password reset poisoning
Changing header Host: from legit to our malicious domain to obtain password reset link which include the unique reset token.   
1.) In Burp "POST /forgot-password" request body change the username parameter and change the Host: header to your exploit server's domain name  
2.) In malicious servers access.log obtain reset token  
3.) In Burp use our acc genuine password reset link from email, but change token parametre in URL.  
>/forgot-password?temp-forgot-password-token=S0j4fkc7y9M9AgAPXu8kDsNG1pT9kPZ4    

### Host header authentication bypass - change "Host: localhost"
If we can change Host: header to any value and still successfully access the home page.  
1.) at robots.txt is an /admin.  
2.) We cant access /admin, theres error message "Admin interface only available to local users"  
3.) In Burp change Host: header to localhost  
> Host: localhost  



