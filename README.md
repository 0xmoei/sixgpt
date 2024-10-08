# sixgpt

* You must have logged into https://sixgpt.xyz with your `wallet` & `email` before running the miner
* Make sure the wallet associated with your vana private key has enough $VANA balance on the desired network (at least 0.1) - [VANA faucet](https://faucet.vana.org/satori)

## Install sixgpt
**1. Create folders**
```
mkdir sixgpt
```
```
cd sixgpt
```

**2. Set Variables - Replace `your_private_key`**
```
export VANA_PRIVATE_KEY=your_private_key
export VANA_NETWORK=satori
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
