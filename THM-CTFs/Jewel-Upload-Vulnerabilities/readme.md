# Upload Vulnerabilities

**URL - jewel.uploadvulns.thm**

### Enumeration
Output of gobuster is [here](./gobuster-output.txt)

Form Wappalyzer we found,
```
Web frameworks - Express
Web servers - Nginx 1.17.6
Programming languages - Node.js
JavaScript libraries - jQuery 3.5.1
Reverse proxies - Nginx 1.17.6
```

### Filtering
- Client Filtering (source code)
    - Extension Filtering
    - File Type Filtering
        - magic number validation
    - Length Filtering (~306 kb)

- Server Filtering
    - File Type Filtering
        - MIME validation

**Script : shell.jpeg**

### Bypassing Filter
From above situation,
- Intercept http get respond (java script one with name upload.js) and comment out magic number validation
- Upload reverse shell script (node js)

Now, we have successfully uploaded our script

### Enumerate Files in content

```bash
gobuster dir -x jpg -u http://jewel.uploadvulns.thm/content -w UploadVulnsWordlist.txt -o file-enum.txt
```

Files that are displayed on webpage (from inspector element)
- /content/ABH.jpg
- /content/LKQ.jpg
- /content/SAD.jpg
- /content/UAD.jpg

From file [files-enum.txt](./files-enum.txt), only image with size 382 bytes is **/CCA.jpg** which is of same size as of our script.

### Reverse shell
Let's run our script
- Head on to http://jewel.uploadvulns.thm/admin/ (find this in our enumeration)
- Since this page only execute content of /modules, so we need to traverse through top level directory using, <br>
```
../content/CCA.jpg
```
To execute our script.

*NOTE : Set up reverse shell listener before executing script*

Now, we get reverse shell access, so head on to /var/www and concatenate flag.txt to get our flag

**FLAG : THM{NzRlYTUwNTIzODMwMWZhMzBiY2JlZWU2}**
