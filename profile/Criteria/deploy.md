### Guia de deploy em vps via ssh
**Configuracao Nginx**
- crie o arquivo: `nomedoapp.conf`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
- acesse o conteudo com: `sudo nano /etc/nginx/nginx.conf`
- coloque o upstream correspondente ao `nomedoapp.conf`
- acesse o zerossl.com e crie um ssl para o front end 
- baixe o .zip extraia e execute o `type ca_bundle.crt >> certificate.crt`
- altere o nome da pasta para nao conter espacos: `nomedoapp`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
  
- copia a pasta `nomedoapp` para a pasta nginx definitiva: 
` sudo mkdir -p /etc/nginx/certificados/nomedoapp && \
sudo cp /home/administrator/etc/nginx/certificados/nomedoapp/certificate.crt /etc/nginx/certificados/nomedoapp/certificate.crt && \
sudo cp /home/administrator/etc/nginx/certificados/nomedoapp/private.key /etc/nginx/certificados/nomedoapp/private.key && \
sudo chown root:root /etc/nginx/certificados/nomedoapp/* && \
sudo chmod 644 /etc/nginx/certificados/nomedoapp/certificate.crt && \
sudo chmod 600 /etc/nginx/certificados/nomedoapp/private.key `


- acesse o zerossl.com e crie um ssl para o back end
- baixe o .zip extraia e execute o `type ca_bundle.crt >> certificate.crt`
- altere o nome da pasta para nao conter espacos: `nomedoapp`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
  
- copia a pasta `apinomedoapp` para a pasta nginx definitiva: 
` sudo mkdir -p /etc/nginx/certificados/apinomedoapp && \
sudo cp /home/administrator/etc/nginx/certificados/apinomedoapp/certificate.crt /etc/nginx/certificados/apinomedoapp/certificate.crt && \
sudo cp /home/administrator/etc/nginx/certificados/apinomedoapp/private.key /etc/nginx/certificados/apinomedoapp/private.key && \
sudo chown root:root /etc/nginx/certificados/apinomedoapp/* && \
sudo chmod 644 /etc/nginx/certificados/apinomedoapp/certificate.crt && \
sudo chmod 600 /etc/nginx/certificados/apinomedoapp/private.key `

- copie o arquivo nomedoapp.conf para a maquina: `scp nomedoapp.conf administrator@IP_DO_SERVIDOR:/home/administrator/etc/nginx/sites-available/`
  
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
- copia o nomedoapp.conf para a pasta nginx definitiva: `sudo cp /home/administrator/etc/nginx/sites-available/nomedoapp.conf /etc/nginx/sites-available/nomedoapp.conf && \
sudo chown root:root /etc/nginx/sites-available/nomedoapp.conf && \
sudo chmod 644 /etc/nginx/sites-available/nomedoapp.conf && \
sudo ln -sf /etc/nginx/sites-available/nomedoapp.conf /etc/nginx/sites-enabled/nomedoapp.conf`


- checka se tudo esta certo: sudo nginx -t && sudo systemctl reload nginx && echo ".conf e certificados instalados"
