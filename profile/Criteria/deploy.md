### Guia de deploy em vps via ssh
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
  docker --version && docker compose version
  ```



- Testar a instalação
  ```bash
  docker --version
  docker compose version
  ```






**Configuracao Nginx**
- crie o arquivo: `nomedoapp.conf`
- acesse a maquina: `ssh administrator@IP_DO_SERVIDOR`
- edite o conteudo com: `sudo nano /etc/nginx/nginx.conf`
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
