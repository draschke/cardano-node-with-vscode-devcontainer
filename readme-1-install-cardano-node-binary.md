# Installing cardano-node Testnet - NetworkMagic: 1097911063 with this VS Code Dev Container

You can follow this [instruction](https://developers.cardano.org/docs/get-started/installing-cardano-node/

## At first you should create your Dev Container

### Install build dependencies

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install automake build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev make g++ tmux git jq wget libncursesw5 libtool autoconf -y
```

### Installing GHC and Cabal

```bash
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

### Open a new shell and run the following command

```bash
ghcup --version
```

### Supported GHC Versions you will find here

https://github.com/input-output-hk/haskell.nix/blob/master/docs/reference/supported-ghc-versions.md

In our case that version: ghcup --version 8.10.7

### After installation, restart your shell/terminal after installing ghcup 

```bash
ghcup install ghc 8.10.7
ghcup set ghc 8.10.7

ghcup install cabal 3.4.0.0
ghcup set cabal 3.4.0.0
```

### Let's create a working directory to store the source-code and builds for the components

- $HOME=>/workspace

```bash
mkdir -p /cardano/cardano-src
cd /cardano/cardano-src
```

### Next, we will download, compile and install libsodium

```bash
cd /cardano/cardano-src
git clone https://github.com/input-output-hk/libsodium
cd libsodium
git checkout 66f017f1
./autogen.sh
./configure
make
sudo make install
```

### Set the environment variables

```bash
echo 'export LD_LIBRARY_PATH="/usr/local/lib:$LD_LIBRARY_PATH"' >> ~/.bashrc
source ~/.bashrc 
echo $LD_LIBRARY_PATH

echo 'export PKG_CONFIG_PATH="/usr/local/lib/pkgconfig:$PKG_CONFIG_PATH"' >> ~/.bashrc
source ~/.bashrc 
echo $PKG_CONFIG_PATH
source $HOME/.bashrc
```

### Once saved, we will then reload your shell profile to use the new variables. We can do that by typing

```bash
source $HOME/.bashrc
```

## download, compile and install cardano-node and cardano-cli

```bash
cd /cardano/cardano-src
```

### Download the cardano-node repository

```bash
cd /cardano/cardano-src
git clone https://github.com/input-output-hk/cardano-node.git
cd cardano-node
git fetch --all --recurse-submodules --tags
```

### Switch the repository to the latest tagged commit

```bash
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
```

### Configuring the build options

```bash
cabal configure --with-compiler=ghc-8.10.7
```

### We can now build the Haskell-based cardano-node to produce executable binaries

```bash
cabal build cardano-node cardano-cli
```

```bash
mkdir -p $HOME/.local/bin
cp -p "$(./scripts/bin-path.sh cardano-node)" $HOME/.local/bin/
cp -p "$(./scripts/bin-path.sh cardano-cli)" $HOME/.local/bin/
```

### Check for version

```bash
$ ghc --version
The Glorious Glasgow Haskell Compilation System, version 8.10.7
```

```bash
$ cabal --version
cabal-install version 3.6.0.0
compiled using version 3.6.1.0 of the Cabal library
```

```bash
$ cardano-node --help
Usage: cardano-node run
```

```bash
$ cardano-cli --help
```

### run get_latest_config_files from root of the repository

```bash
vscode ➜ /workspace (master ✗) 
$ ./get_latest_config_files.sh 
```

### Testnet / Sandbox​ - NetworkMagic: 1097911063

```bash
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-topology.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-shelley-genesis.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-config.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-byron-genesis.json
curl -O -J https://hydra.iohk.io/build/7654130/download/1/testnet-alonzo-genesis.json
```

### After the end the root should look like this

vscode ➜ /workspace (master ✗) $ ls -la
total 44
drwxr-xr-x 7 vscode vscode 4096 Dec 13 10:18 .
drwxr-xr-x 1 root   root   4096 Dec 12 11:42 ..
drwxr-xr-x 4 vscode vscode 4096 Dec 12 13:34 cardano-src
drwxr-xr-x 3 vscode vscode 4096 Dec 12 11:30 .devcontainer
-rwxr-xr-x 1 vscode vscode 1466 Dec 12 16:27 get_latest_config_files.sh
drwxr-xr-x 8 vscode vscode 4096 Dec 13 10:18 .git
-rw-r--r-- 1 vscode vscode    0 Dec 13 10:16 .gitignore
-rw-r--r-- 1 vscode vscode 3196 Dec 13 10:15 readme-1-install-cardano-node-binary.md
-rw-r--r-- 1 vscode vscode 3463 Dec 13 10:18 readme-2-running-cardano-node.md
drwxr-xr-x 3 vscode vscode 4096 Dec 12 16:32 relay

### relay directory looks like this

```bash
vscode ➜ /workspace (master ✗) 
$ cd relay/ (master ✗) $ ls -la
total 68
drwxr-xr-x 3 vscode vscode  4096 Dec 11 11:09 .
drwxr-xr-x 6 vscode vscode  4096 Dec 11 11:08 ..
drwxr-xr-x 5 vscode vscode  4096 Dec 11 11:16 db
-rw-r--r-- 1 vscode vscode  9459 Dec 11 11:09 testnet-alonzo-genesis.json
-rw-r--r-- 1 vscode vscode 31112 Dec 11 11:09 testnet-byron-genesis.json
-rw-r--r-- 1 vscode vscode  2738 Dec 11 11:09 testnet-config.json
-rw-r--r-- 1 vscode vscode  2487 Dec 11 11:09 testnet-shelley-genesis.json
-rw-r--r-- 1 vscode vscode   131 Dec 11 11:09 testnet-topology.json
```

## If your server has only 8G of RAM you should configure the SWAP file

### Configuration works only in WSL (Open another window in WSL and change the file)

```bash
free -m
vscode ➜ ~ $ free -m
               total        used        free      shared  buff/cache   available
Mem:            7959        5559         116         365        2284        1740
Swap:              0           0           0
```

### next command will alocate 3G for SWAP file (you can configure what value you want - just replace 3G with your needs)

```bash
sudo fallocate -l 3G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
now to make it permanently type
sudo nano /etc/fstab
```

### and add to the end, as a new line

/swapfile swap swap defaults 0 0

### and save the file (Ctrl + x then Y then ENTER)

### check if the configuration was successfully

```bash
➜  diraschk free -m
              total        used        free      shared  buff/cache   available
Mem:           7959        3687        1676         365        2594        3611
Swap:          3071           0        3071
```
