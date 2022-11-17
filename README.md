# Web-app-testing-list

## HTTP Host header attacks ====================   
### Password reset poisoning
We visit https://example.com/security, browser will compose a request containing a Host header:
> GET /security HTTP/1.1  
> Host: example.com  

When a browser sends the request, URL will resolve to the IP address of server. When server receives the request, it refers to the Host header to determine the intended back-end and forwards the request accordingly to us.  

* **Password reset poisoning** - changing header Host: from legit to our malicious domain to obtain password reset link which include the unique reset token. 
1.) In Burp "POST /forgot-password" request contains the username as a body parameter  
2.) Change the Host: header to your exploit server's domain name, at malicious servers access.log obtain reset token  
3.) In Burp use genuine password reset and change token parametre in URL.  
>/forgot-password?temp-forgot-password-token=S0j4fkc7y9M9AgAPXu8kDsNG1pT9kPZ4  


