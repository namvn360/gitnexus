#demo.com
cd ~/project folder

chmod +x ./prod or dev
then run: ./prod or dev
Repo for demo
generate ssl:
#Set environment variables for dev
export DOMAIN=${DOMAIN:-demo.com}

certbot certonly --standalone --quiet --agree-tos -m admin${DOMAIN} -d ${DOMAIN} -d www.${DOMAIN} --pre-hook "service nginx stop" --post-hook "service nginx start"
#crontab
0 0 * * 0 certbot renew --pre-hook "service nginx stop" --post-hook "service nginx start"
ln -fs /home/${DOMAIN}/config/nginx.conf /etc/nginx/sites-enabled/${DOMAIN}.conf

#restart nginx
service nginx restart