---
icon: globe
description: >-
  The FTP module allows you perform various FTP and SFTP operations against a
  remote server.
---

# FTP

```javascript
// Open connection to FTP server, calling it 'conn'
bx:ftp
    action="open"
    connection="conn"
    server="ftp.server.com"
    port="21"
    username="user"
    password="password"
    passive="true";
    
// List the current directory
bx:ftp
    action="listDir"
    connection="conn"
    name="result";

// Dump the results
bx:dump var="result";
```



## Available Actions

Here is the list of available actions you can use.

| Action        | Description |
| ------------- | ----------- |
| changeDir     |             |
| close         |             |
| createDir     |             |
| existsDir     |             |
| existsFile    |             |
| getCurrentDir |             |
| getFile       |             |
| listDir       |             |
| open          |             |
| putfile       |             |
| removeDir     |             |
| remove        |             |

## Examples

### Open FTP Connection

```
bx:ftp
    action="open"
    connection="conn"
    username="user"
    password="password"
    server="ftp.server.com"
    port="21"
    passive="true|false";
```

### Open SFTP Connection

To connect using SFTP, you need to set `secure`equal to `true`.

```
bx:ftp
    action="open"
    connection="conn"
    username="user"
    password="password"
    server="server.com"
    port="22"
    passive="true|false"
    secure="true";
```

### Close Connection

```
bx:ftp action="close" connection="conn" result="myResult";
```

