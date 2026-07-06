## NGINX 
### Instalar NGINX en Ubuntu 24.04.4 LTS
```
sudo apt install nginx -y
sudo nginx -v   (revisar version)
sudo nginx -t   (revisar la sintaxis de los archivos de configuracion)
sudo systemctl status nginx
sudo systemctl enable nginx


uti@srv-test-nginx:/var/www/html$ cat index.nginx-debian.html (la pagina por defecto)
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

uti@srv-test-nginx:~$ ls /var/log/nginx/  (en esa ruta etan los logs para el acceso y los errores)

uti@srv-test-nginx:~$ sudo tail -f /var/log/nginx/access.log
uti@srv-test-nginx:~$ sudo tail -f /var/log/nginx/error.log


```

