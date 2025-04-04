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
