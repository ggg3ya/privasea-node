# Privanetix node

## 1. Minimum system requirements
- OS: Ubuntu (Recommended)
- The CPU can only be amd64 architecture.
- Storage: 100GB available storage
- Memory: 4GB RAM
- Processor: A processor with 6 cores, x86 architecture.

## 2. Install Docker
`If Docker is already installed, you can skip this step.`

#### 1. Installation Commands

- Install necessary dependencies
```
sudo apt update && sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
```

- Add Docker's official GPG key
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
- Add Docker's official repository
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
- Update APT package index
```
sudo apt update
```
- Install Docker
```
sudo apt install -y docker-ce
```

#### 2. Verify if Docker is installed successfully
```
sudo docker --version
```

#### 3. Start and enable Docker service
```
sudo systemctl start docker
sudo systemctl enable docker
```
## 3. Pull docker mirroring
First pull the docker image using the following command:
```
docker pull privasea/acceleration-node-beta:latest
```

## 4. Node program configuration
#### 1. Run the node program as the root user:
```
sudo su
```

#### 2. Create the program running directory and navigate to it:
```
mkdir -p  /privasea/config && cd  /privasea
```

#### 3. Get the keystore file

Use an existing wallet keystore file or execute the following command to generate a new one:

```
docker run -it -v "/privasea/config:/app/config"  \
privasea/acceleration-node-beta:latest ./node-calc new_keystore
```
#### Note: The program will prompt you to enter a password, `please remember this password for future use.` The generated keystore file will have a corresponding `node address`, please save this address, it will be used in the dashboard configuration

After successful creation of the wallet, the program will display information similar to the following:

`node address: 0xf07c3eF23ae7BEb8CD8bA5fF546E35Fd4b332B34`
# This is the node address you generated, used for linking in the dashboard 
node filename: keystore:///app/config/UTC--2025-01-06T06-11-07.485797065Z--f07c3`

#### 4. Rename the keystore file in the/privasea/config folder to wallet_keystore:

Check if there is a keystore file in the /privasea/config directory:
```
cd /privasea/config && ls
```

Rename the keystore file you noted in the previous step:
```
mv ./UTC--2025-01-06T06-11-07.485797065Z--f07c3ef23ae7beb8cd8ba5ff546e35fd4b332b34  ./wallet_keystore
```

Replace UTC--2025-01-06T06-11-07.485797065Z--f07c3ef23ae7beb8cd8ba5ff546e35fd4b332b34 with the name of the keystore file you found.

Check that the wallet_keystore file in the /privasea/config folder was modified correctly:
```
ls
```

#### 5. Link node address and reward address
Use the wallet address corresponding to the keystore file to link it with the reward address on [DeepSea dashboard](https://deepsea-beta.privasea.ai/privanetixNode)


#### 6. 6. Start the node
Run the command to start the Privanetix(acceleration) node:

- Switch to the program running directory
```
cd /privasea/
```
- Run the compute node command:
```
docker run  -d   -v "/privasea/config:/app/config" \
  -e KEYSTORE_PASSWORD=123456 \
  privasea/acceleration-node-beta:latest
```
 
Parameter Explanation:
- `EYSTORE_PASSWORD`: The password corresponding to the keystore file
- `/privasea/config`: The local directory where the wallet_keystore file is stored

## 5. Stop the node
```
docker ps -q --filter "ancestor=privasea/acceleration-node-beta:latest" | xargs --no-run-if-empty docker stop
```

 `Run the command below; if there is no output, it indicates that the node has been stopped.`
```
docker ps | grep privasea/acceleration-node-beta:latest
```
