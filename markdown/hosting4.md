# Care and feeding of your webserver

logs

in 

sudo vi /etc/nginx/sites-available/brandonrohrer.com

$uri.html 

try_files $uri $uri/ =404;

try_files $uri $uri.html $uri/ =404;

sudo nginx -t

sudo systemctl restart nginx

