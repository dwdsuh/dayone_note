# How to Use 'Curl' Command in Linux



## 1. check version

```bash
curl --version
```



## 2. Download a file

```bash
curl -O <url>
curl -o <file-name> <url>
```



## 3. Resume an Interrupted Download

```bash
curl -C - -O <url>
```



## 4. Download multiple files

```bash
curl -O <url> -O <url>
```



## 5. Download Urls from a file

```bash
xargs -n 1 curl -O < url_list.txt
```



## 6. Use a Proxy with or without Authentication

```bash
curl -x proxy.yourdomain.com:8080 -U user:password -O http://yourdomain.com/yourfile.tar.gz
```

