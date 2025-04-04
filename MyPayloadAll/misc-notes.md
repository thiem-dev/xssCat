## Things to Try

- cookiejar overflow: https://medium.com/@ibm_ptc_security/cookie-jar-overflow-attack-ae5135b6100
  - fill the app with so many junk cookies (past browser limit) it overwrites real cookies with ones that don't have the HTTPOnly flag
    - this is better in conjunction with xss

- Get company internal IPs via email (use the contact pages): https://medium.com/@far00t01/postmaster-thanks-for-the-internal-ip-2fcc1f4e541e 

- Basic IIS misconfigurations: https://medium.com/@far00t01/asp-net-microsoft-iis-pentesting-04571fb071a4
  - use : `GET /a.aspx HTTP/1.1` 

- Hyperlink injection: https://medium.com/@far00t01/hyperlink-injection-an-underrated-vulnerability-29e93bbd967b

- server-sided XSS: https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/server-side-xss-dynamic-pdf
    - can even read files

- prototype pollution: 
  - look for reflections in POST requests of objects. find object property names
  - add tye proto, and then try and break the json above it and see if the response reflects your input
      ```js

      //first try
      {
        "name":"sampleData",
        "isAdmin":true,
        "__proto__": {
          "status":500
        }
      }

      //second try
      {
        "name":"sampleData",
        "__proto__": {
          "status":500,
          "isAdmin":true
        }
      }

      //one liner
      user={__proto__:{toString:function(){alert("Prototype Pollution Detected")}}};
      ```
