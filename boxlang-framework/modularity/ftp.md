---
icon: globe
description: >-
  The FTP module allows you perform various FTP and SFTP operations against a
  remote server.
---

# FTP

```javascript
bx:ftp
    action="open"
    connection="conn"
    server="ftp.server.com"
    port="21"
    username="user"
    password="password"
    passive="true";
    
bx:ftp
    action="listDir"
    connection="conn"
    name="result";
    
bx:dump var="Result";
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

### Open Connection

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

### Close Connection

```
bx:ftp action="close" connection="conn" result="myResult";
```

