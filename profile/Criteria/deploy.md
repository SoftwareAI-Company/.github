
no windows:
scp empl.conf administrator@IP_DO_SERVIDOR:/home/administrator/etc/nginx/sites-available/

ssh administrator@IP_DO_SERVIDOR

sudo cp /home/administrator/etc/nginx/sites-available/employersai.conf /etc/nginx/sites-available/employersai.conf && \
sudo chown root:root /etc/nginx/sites-available/employersai.conf && \
sudo chmod 644 /etc/nginx/sites-available/employersai.conf && \
sudo ln -sf /etc/nginx/sites-available/employersai.conf /etc/nginx/sites-enabled/employersai.conf && \

sudo mkdir -p /etc/nginx/certificados/apiemployersai && \
sudo cp /home/administrator/etc/nginx/certificados/apiemployersai/certificate.crt /etc/nginx/certificados/apiemployersai/certificate.crt && \
sudo cp /home/administrator/etc/nginx/certificados/apiemployersai/private.key /etc/nginx/certificados/apiemployersai/private.key && \
sudo chown root:root /etc/nginx/certificados/apiemployersai/* && \
sudo chmod 644 /etc/nginx/certificados/apiemployersai/certificate.crt && \
sudo chmod 600 /etc/nginx/certificados/apiemployersai/private.key && \


sudo nginx -t && sudo systemctl reload nginx && echo ".conf e certificados instalados"
