# sixgpt

## Before start:
* You must have logged into https://sixgpt.xyz with your `wallet` & `email` before running the miner
* Make sure the wallet associated with your vana private key has enough $VANA balance on the desired network (at least 0.1) - [VANA faucet](https://faucet.vana.org/satori)

## Install Docker
```console
# Docker
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo chmod +x /usr/local/bin/docker-compose

# Docker version check
docker --version
```

## Install sixgpt
**1. Create folders**
```
mkdir sixgpt
```
```
cd sixgpt
```

**2. Set Variables - Replace `your_private_key`**
```console
export VANA_PRIVATE_KEY=your_private_key
export VANA_NETWORK=satori

# you can use moksha network too
export VANA_NETWORK=moksha
```

**3. Create docker-compose.yml**
```
nano docker-compose.yml
```
Enter the following code in it
```
version: '3.8'

services:
  ollama:
    image: ollama/ollama:0.3.12
    ports:
      - "11435:11434"
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped
 
  sixgpt3:
    image: sixgpt/miner:latest
    ports:
      - "3015:3000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=${VANA_PRIVATE_KEY}
      - VANA_NETWORK=${VANA_NETWORK}
    restart: always

volumes:
  ollama:
```
To save: `CTRL+X+Y`+`ENTER`

## Start miner
```
docker compose up -d
```

## Logs
```
docker compose logs -fn 100
```
![image](https://github.com/user-attachments/assets/4d36e28c-c260-4727-8ff7-1c57312bc769)


## Mine on 2 Networks (Moksha + Satori)
1. Directory
```
cd sixgpt
```
2. Stop
```
docker compose down
```
3. Edit docker compose
```
nano docker-compose.yml
```
4. Replace:
```
version: '3.8'

services:
  ollama:
    image: ollama/ollama:0.3.12
    ports:
      - "11435:11434"
    volumes:
      - ollama:/root/.ollama
    restart: unless-stopped
 
  sixgpt3-satori:
    image: sixgpt/miner:latest
    ports:
      - "3015:3000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=${VANA_PRIVATE_KEY}
      - VANA_NETWORK=satori
    restart: always

  sixgpt3-moksha:
    image: sixgpt/miner:latest
    ports:
      - "3016:3000"
    depends_on:
      - ollama
    environment:
      - VANA_PRIVATE_KEY=${VANA_PRIVATE_KEY}
      - VANA_NETWORK=moksha
    restart: always

volumes:
  ollama:
```

5. Run Miner
```console
# Replace your_private_key
export VANA_PRIVATE_KEY=your_private_key

docker compose up -d
```
