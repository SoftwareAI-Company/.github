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



