# nginx: Basics

A basic introduction to **nginx**, how to manage its lifecycle, configure it, serve static content, set up a proxy, and configure FastCGI proxying.

---
## **Overview**

> nginx is a high-performance HTTP server, reverse proxy, and IMAP/POP3 proxy server.

* One **master process** manages configuration and worker processes
* **Worker processes** handle actual client requests
* Configuration is event-based and highly efficient
* Configuration file is usually named nginx.conf

* Default config locations:

  * `/etc/nginx`
  * `/usr/local/nginx/conf`
  * `/usr/local/etc/nginx`

---

# **Managing nginx**

## **Starting and Stopping nginx**

```nginx
nginx        # start nginx
nginx -s stop    # fast shutdown
nginx -s quit    # graceful shutdown (waits for requests to finish)
nginx -s reload  # reload configuration
nginx -s reopen  # reopen log files
```

> All commands must be run by the same user who started nginx.

## **Alternative: Using**

## **kill**

Find nginx master process:

```bash
ps -ax | grep nginx
```

Send signal:

```bash
kill -s QUIT <pid>
```

Default PID location:

* `/usr/local/nginx/logs/nginx.pid`
* `/var/run/nginx.pid`

---
## **Configuration File Structure**

### **Directive Types**

|  **Type**  |  **Description**  | 
|---|---|
|  **Simple**  |  Ends with ;, e.g. worker_processes 4;  | 
|  **Block**  |  Has { ... }, can contain nested directives  | 

### **Contexts**

|  **Context**  |  **Can contain**  | 
|---|---|
|  **main**  |  events, http  | 
|  **http**  |  server  | 
|  **server**  |  location  | 
|  **location**  |  Actual routing rules  | 

> Lines starting with # are comments.

---
## **Serving Static Content**

### **Directory Setup**

```bash
mkdir -p /data/www
echo "Hello World" > /data/www/index.html

mkdir -p /data/images
# Add image files here
```

### **nginx Config**

```nginx
http {
    server {
        location / {
            root /data/www;
        }

        location /images/ {
            root /data;
        }
    }
}
```

> URI `/images/example.png` → `/data/images/example.png`

## **Apply Changes**

```bash
nginx -s reload
```

> Check logs if things go wrong:

* Access log: `/var/log/nginx/access.log`
* Error log: `/var/log/nginx/error.log`

---
## **Setting Up a Simple Proxy Server**

### **Create Upstream Server**

```nginx
server {
    listen 8080;
    root /data/up1;

    location / {
        # serves /data/up1/index.html
    }
}
```

### **Create Proxy Server**

```nginx
server {
    location / {
        proxy_pass http://localhost:8080/;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

> Requests:

* /images/foo.png → /data/images/foo.png
* /about.html → proxied to localhost:8080

### **Matching Logic**

* nginx checks **prefix locations** first
* Then checks **regex locations**
* Chooses longest matching prefix or matching regex

---
## **FastCGI Proxying**

### **Setup**

```nginx
server {
    location / {
        fastcgi_pass  localhost:9000;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param QUERY_STRING    $query_string;
    }

    location ~ \.(gif|jpg|png)$ {
        root /data/images;
    }
}
```

> Useful for PHP or other apps using FastCGI (e.g., via PHP-FPM)

---
## **Summary**

* nginx uses a modular, hierarchical configuration
* Master process controls workers, workers handle requests
* Static files are served via root
* Proxies are defined with proxy_pass or fastcgi_pass
* Use location blocks and regex for request routing
* Reloading config is safe and does not drop current connections

---

#backend