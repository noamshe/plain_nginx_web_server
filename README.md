Thanks to:
http://www.thegeekstuff.com/2011/07/install-nginx-from-source/

1. Download nginx

Download nginx from here, or use wget as shown below. The current stable version is nginx 1.0.5

cd
wget http://nginx.org/download/nginx-1.0.5.tar.gz
tar xvfz nginx-1.0.5.tar.gz
cd nginx-1.0.5
2. Install nginx
There are lot of options that you can pass to ./configure. To identify list of all the configuration options do the following.

./configure --help
./configure
make
make install


vi /usr/local/nginx/conf/nginx.conf
    server {
        listen       8081;
        server_name  localhost;
4. Start Nginx Server

nginx executable is located under /usr/local/nginx/sbin directory. Just call this executable to start the nginx server.

cd /usr/local/nginx/sbin
./nginx
Once you start this, you’ll see the nginx “master process” and “worker process” if you do ps.

# ps -ef | grep -i nginx
root     18596 13:16 nginx: master process ./nginx
nobody   18597 13:16 nginx: worker process
After you start the nginx server, go to http://your-ip-address/ (or http://your-ip-address:8081, if you changed the listen directive in nginx.conf), you should see the default nginx index.html, which should say “Welcome to nginx!”

5. Stop Nginx Server

To stop the nginx server, do the following.

cd /usr/local/nginx/sbin
./nginx -s stop
To view the current version of nginx, do the following:

# ./nginx -v
nginx: nginx version: nginx/1.0.5
To debug issues, view the error.log and access.log files located under /usr/local/nginx/logs

# ls /usr/local/nginx/logs/
access.log
error.log
nginx.pid




./configure --without-http_rewrite_module --without-http_gzip_module --add-module=/home/noam/myapps/helloNGinx

noam@linux-noam:/data/nginx-1.0.5$ sudo vim /usr/local/nginx/conf/nginx.conf
noam@linux-noam:/data/nginx-1.0.5$ /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
nginx: [alert] could not open error log file: open() "/usr/local/nginx/logs/error.log" failed (13: Permission denied)
2014/04/24 16:03:16 [emerg] 20767#0: open() "/usr/local/nginx/logs/access.log" failed (13: Permission denied)
noam@linux-noam:/data/nginx-1.0.5$ sudo !!
sudo /usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf
noam@linux-noam:/data/nginx-1.0.5$ 




NGinx Load Balancer:
sudo vim /usr/local/nginx/conf/nginx.conf

upstream nodejs {
      ip_hash;
      server localhost:8080 max_fails=3 fail_timeout=30s; # Reverse proxy to machine-1
      server localhost:8081 max_fails=3 fail_timeout=30s; # Reverse proxy to machine-2
}

server {
      listen       8090;
      server_name  localhost;
      #charset koi8-r;
      #access_log  logs/host.access.log  main;

      location / {
          proxy_pass http://nodejs; # Load balance the URL location "/" to the upstream lb-subprint
          proxy_set_header Connection 'upgrade';
          proxy_set_header Host $host;
      }
...
}



