### Guia de criacao de ssl gratis atraves do lets encrypt


Você já **emitiu o certificado com sucesso**, agora falta **ativar ele no Nginx** e **recarregar o serviço**.

Aqui está exatamente o que fazer agora:

---

## 1️⃣ Verifique se existe um servidor Nginx para `mediacutsstudio.com`

Execute:

```bash
sudo nano /etc/nginx/sites-available/mediacutsstudio.conf
```

---

## 2️⃣ Configure o HTTPS usando o certificado recém-criado

No seu server block `server {}` da porta **443**, coloque o novo certificado:

```nginx
server {
    listen 443 ssl;
    listen [::]:443 ssl;

    server_name mediacutsstudio.com www.mediacutsstudio.com;

    ssl_certificate /etc/letsencrypt/live/mediacutsstudio.com/fullchain.pem; 
    ssl_certificate_key /etc/letsencrypt/live/mediacutsstudio.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;

    root /var/www/mediacutsstudio.com/html;
    index index.html index.htm index.nginx-debian.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```


## 3️⃣ Teste configuração

```bash
sudo nginx -t
```

Se estiver tudo ok:

---

## 4️⃣ Reinicie o Nginx

```bash
sudo systemctl reload nginx
```

---

## 5️⃣ Verificar se está funcionando

```bash
curl -I https://mediacutsstudio.com
```

Se aparecer `200` ou `301`, está ativo.

---

## 6️⃣ Renovação automática (provavelmente já está ativa)

Cheque:

```bash
sudo systemctl status certbot.timer
```

Se estiver `active`, a renovação está configurada.

---




Tudo certo com o certificado do domínio — ele já está ativo e funcionando.
Agora vamos **habilitar a renovação automática**, porque seu sistema **não tem o `certbot.timer` instalado** (instalações antigas do Ubuntu usam outro método).

Vou te guiar direto ao ponto sem enrolação:

---

# ✅ 1️⃣ Verificar se existe o cron do Certbot

Execute:

```bash
sudo systemctl status certbot.service
```

Se **não existir**, prossiga para instalar o serviço de renovação corretamente.

---

# ✅ 2️⃣ Criar cron de renovação (garante 100% que renova sozinho)

Crie o cron:

```bash
sudo nano /etc/cron.d/certbot
```

Cole:

```
0 */12 * * * root certbot renew --quiet
```

Salvar e sair.

Esse cron roda **duas vezes por dia** e renova automaticamente.

---

# ✅ 3️⃣ Garantir que Nginx reinicia quando renovar

Editar hook do Certbot:

```bash
sudo nano /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh
```

Conteúdo:

```bash
#!/bin/bash
systemctl reload nginx
```

Dar permissão:

```bash
sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh
```

---

# ✅ 4️⃣ Testar a renovação agora

```bash
sudo certbot renew --dry-run
```

Se aparecer **"Congratulations, all simulated renewals succeeded"**, está tudo 100%.

---

# 🎯 Agora seu certificado vai renovar automaticamente sem timer.

Se quiser, posso também **configurar todos os 18 domínios de uma vez com SSL** sem quebrar nada. Basta dizer.


exemplo real:
administrator@vps-trbg:~$ sudo certbot certonly --nginx
Saving debug log to /var/log/letsencrypt/letsencrypt.log

Which names would you like to activate HTTPS for?
We recommend selecting either all domains, or all domains in a VirtualHost/server block.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: employers-ai.site
2: api.employers-ai.site
3: www.api.employers-ai.site
4: www.employers-ai.site
5: mediacutsstudio.com
6: api.mediacutsstudio.com
7: chatkit.api.mediacutsstudio.com
8: www.chatkit.api.mediacutsstudio.com
9: www.api.mediacutsstudio.com
10: api.sheduler.mediacutsstudio.com
11: www.api.sheduler.mediacutsstudio.com
12: webhook.mediacutsstudio.com
13: www.webhook.mediacutsstudio.com
14: www.mediacutsstudio.com
15: portifolio-ualerson.site
16: api.portifolio-ualerson.site
17: www.api.portifolio-ualerson.site
18: www.portifolio-ualerson.site
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 5
Requesting a certificate for mediacutsstudio.com

Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/mediacutsstudio.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/mediacutsstudio.com/privkey.pem
This certificate expires on 2026-03-03.
These files will be updated when the certificate renews.

NEXT STEPS:
- The certificate will need to be renewed before it expires. Certbot can automatically renew the certificate in the background, but you may need to take steps to enable that functionality. See https://certbot.org/renewal-setup for instructions.

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
If you like Certbot, please consider supporting our work by:
 * Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
 * Donating to EFF:                    https://eff.org/donate-le
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
administrator@vps-trbg:~$ sudo nano /etc/nginx/sites-available/mediacutsstudio.com
administrator@vps-trbg:~$ ls /etc/nginx/sites-available
default  employersai.conf  mediacutsstudio.conf  portfolio.conf
administrator@vps-trbg:~$  sudo nano /etc/nginx/sites-available/mediacutsstudio.conf
administrator@vps-trbg:~$ ls /etc/nginx/sites-available
default  employersai.conf  mediacutsstudio.conf  portfolio.conf
administrator@vps-trbg:~$ sudo nano /etc/nginx/sites-available/mediacutsstudio.com
administrator@vps-trbg:~$ sudo nano /etc/nginx/sites-available/mediacutsstudio.conf
administrator@vps-trbg:~$ administrator@vps-trbg:~$ sudo nano /etc/nginx/sites-available/mediacutsstudio.conf
administrator@vps-trbg:~$ administrator@vps-trbg:~$ sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
administrator@vps-trbg:~$ sudo systemctl reload nginx
administrator@vps-trbg:~$ curl -I https://mediacutsstudio.com
HTTP/1.1 200 OK
Server: nginx/1.18.0 (Ubuntu)
Date: Wed, 03 Dec 2025 02:06:42 GMT
Content-Type: text/html
Connection: keep-alive
Vary: Origin
Cache-Control: no-cache
Etag: W/"554-CSOaAxLMh+ugGP5NasG67qTuo0g"

administrator@vps-trbg:~$ sudo systemctl status certbot.timer
Unit certbot.timer could not be found.
administrator@vps-trbg:~$ sudo systemctl status certbot.service
Unit certbot.service could not be found.
administrator@vps-trbg:~$ sudo nano /etc/cron.d/certbot
administrator@vps-trbg:~$ administrator@vps-trbg:~$ sudo nano /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh
administrator@vps-trbg:~$ sudo chmod +x /etc/letsencrypt/renewal-hooks/deploy/reload-nginx.sh
administrator@vps-trbg:~$ sudo certbot renew --dry-run
Saving debug log to /var/log/letsencrypt/letsencrypt.log

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Processing /etc/letsencrypt/renewal/mediacutsstudio.com.conf
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Account registered.
Simulating renewal of an existing certificate for mediacutsstudio.com

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations, all simulated renewals succeeded:
  /etc/letsencrypt/live/mediacutsstudio.com/fullchain.pem (success)
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -




### Guia de deploy em vps via ssh
**Configuração xRDP**
O xRDP permite acessar a VPS via desktop remoto (RDP).
- acesse a maquina:
  ```bash
  ssh administrator@IP_DO_SERVIDOR
  ```
- Instalar xRDP e ambiente gráfico

```bash
sudo apt update && sudo apt upgrade -y  && \
sudo apt install -y xfce4 xfce4-goodies xrdp  && \
echo "startxfce4" > ~/.xsession
```

- Habilitar e iniciar o serviço xRDP

```bash
sudo systemctl enable xrdp  && \
sudo systemctl start xrdp  && \
sudo systemctl status xrdp
```

- Configurar firewall pra xrdp
```bash
sudo apt update -y && sudo apt install ufw -y
sudo ufw allow 3389/tcp  && \
sudo ufw reload
```

---
**Configuracao Docker**
- acesse a maquina:
  ```bash
  ssh administrator@IP_DO_SERVIDOR
  ```
- instala as dependencias:
  ```bash
  sudo apt update && sudo apt upgrade -y && \
  sudo apt install -y ca-certificates curl gnupg lsb-release && \
  sudo mkdir -p /etc/apt/keyrings && \
  curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg && \
  echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null && \
  sudo apt update && \
  sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin && \
  sudo systemctl enable docker && sudo systemctl start docker && \
  docker --version && docker compose version && \
  sudo usermod -aG docker administrator

  ```

- Crie a rede docker
```bash
docker network create rede_externa
```


- Testar a instalação
  ```bash
  docker --version
  docker compose version
  ```






**Configuracao Nginx**
- acesse a maquina:
  ```bash
  ssh administrator@IP_DO_SERVIDOR
  ```
- Instalar Nginx na VPS:
  ```bash
  sudo apt update && sudo apt upgrade -y && \
  sudo apt install -y nginx && \
  sudo systemctl enable nginx && \
  sudo systemctl start nginx && \
  sudo systemctl status nginx
  ```

- crie o arquivo: `nomedoapp.conf`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
- edite o conteudo com:
  ```bash
  sudo nano /etc/nginx/nginx.conf
  ```
- coloque o upstream correspondente ao `nomedoapp.conf`
- acesse o zerossl.com e crie um ssl para o front end 
- baixe o .zip extraia e execute o 
```bash
type ca_bundle.crt >> certificate.crt
```
- altere o nome da pasta para nao conter espacos: `nomedoapp`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
- inicialize a pasta de certificados ssl
```bash
sudo mkdir -p /etc/nginx/certificados && \
sudo mkdir -p /home/administrator/etc/nginx/certificados && \
sudo chown -R administrator:administrator /home/administrator/etc/nginx/ && \
sudo chown -R administrator:administrator /etc/nginx/
```

- abra o cmd na pasta download e copia a pasta `nomedoapp` para a vps:
```bash
scp -r nomedoapp administrator@IP_DO_SERVIDOR:/home/administrator/etc/nginx/certificados/
```

- copia a pasta `nomedoapp` para a pasta nginx definitiva: 
```bash
sudo mkdir -p /etc/nginx/certificados/nomedoapp && \
sudo cp /home/administrator/etc/nginx/certificados/nomedoapp/certificate.crt /etc/nginx/certificados/nomedoapp/certificate.crt && \
sudo cp /home/administrator/etc/nginx/certificados/nomedoapp/private.key /etc/nginx/certificados/nomedoapp/private.key && \
sudo chown root:root /etc/nginx/certificados/nomedoapp/* && \
sudo chmod 644 /etc/nginx/certificados/nomedoapp/certificate.crt && \
sudo chmod 600 /etc/nginx/certificados/nomedoapp/private.key 
```



- acesse o zerossl.com e crie um ssl para o back end
- baixe o .zip extraia e execute o
  ```bash
  type ca_bundle.crt >> certificate.crt
  ```
- altere o nome da pasta para nao conter espacos: `nomedoapp`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
  - inicialize a pasta de certificados ssl
```bash
sudo mkdir -p /etc/nginx/certificados && \
sudo mkdir -p /home/administrator/etc/nginx/certificados && \
sudo chown -R administrator:administrator /home/administrator/etc/nginx/ && \
sudo chown -R administrator:administrator /etc/nginx/
```

- abra o cmd na pasta download e copia a pasta `nomedoapp` para a vps:
```bash
scp -r nomedoapp administrator@IP_DO_SERVIDOR:/home/administrator/etc/nginx/certificados/
```

- copia a pasta `apinomedoapp` para a pasta nginx definitiva: 
```bash
sudo mkdir -p /etc/nginx/certificados/apinomedoapp && \
sudo cp /home/administrator/etc/nginx/certificados/apinomedoapp/certificate.crt /etc/nginx/certificados/apinomedoapp/certificate.crt && \
sudo cp /home/administrator/etc/nginx/certificados/apinomedoapp/private.key /etc/nginx/certificados/apinomedoapp/private.key && \
sudo chown root:root /etc/nginx/certificados/apinomedoapp/* && \
sudo chmod 644 /etc/nginx/certificados/apinomedoapp/certificate.crt && \
sudo chmod 600 /etc/nginx/certificados/apinomedoapp/private.key
```

- crie a pasta de sites disponiveis:
```bash
sudo mkdir -p /etc/nginx/sites-available/ && \
sudo mkdir -p /home/administrator/etc/nginx/sites-available/ && \
sudo chown root:root /etc/nginx/sites-available/* && \
sudo chown root:root /etc/nginx/sites-available/*
```

- copie o arquivo nomedoapp.conf para a maquina:
```bash
scp nomedoapp.conf administrator@IP_DO_SERVIDOR:/home/administrator/
```
hub.conf


- copia o nomedoapp.conf para a pasta nginx definitiva:
```bash
sudo mv /home/administrator/softwareaihub.conf /home/administrator/etc/nginx/sites-available/softwareaihub.conf && \
sudo chown root:root /home/administrator/etc/nginx/sites-available/softwareaihub.conf && \
sudo cp /home/administrator/etc/nginx/sites-available/softwareaihub.conf /etc/nginx/sites-available/softwareaihub.conf && \
sudo chown root:root /etc/nginx/sites-available/softwareaihub.conf && \
sudo chmod 644 /etc/nginx/sites-available/softwareaihub.conf && \
sudo ln -sf /etc/nginx/sites-available/softwareaihub.conf /etc/nginx/sites-enabled/softwareaihub.conf
```

- checka se tudo esta certo:
```bash
sudo nginx -t && sudo systemctl reload nginx && echo ".conf e certificados instalados"
```


**Configuracao de secrets**
- criando o SSH_PRIVATE_KEY
```bash
ssh-keygen -t rsa -b 4096 -C "ServidorVPS"
```
copie a chave que esta no arquivo .pub para authorized_keys


dministrator@vps-trbg:~$ sudo chown -R administrator:administrator /home/administrator/deploys/docsphere
administrator@vps-trbg:~$ sudo chmod -R u+rwX /home/administrator/deploys/docsphere



