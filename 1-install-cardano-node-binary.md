# Installing cardano-node and cardano-cli from source

You can follow this [instruction](https://developers.cardano.org/docs/get-started/installing-cardano-node/

## With vscode user

### Install build dependencies

```bash
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install automake build-essential pkg-config libffi-dev libgmp-dev libssl-dev libtinfo-dev libsystemd-dev zlib1g-dev make g++ tmux git jq wget libncursesw5 libtool autoconf -y
```

### Installing GHC and Cabal

```bash
curl --proto '=https' --tlsv1.2 -sSf https://get-ghcup.haskell.org | sh
```

### Supported GHC Versions you will find here:

https://github.com/input-output-hk/haskell.nix/blob/master/docs/reference/supported-ghc-versions.md

In our case that version: ghcup --version 8.10.7

```bash
ghcup install ghc 8.10.7
ghcup set ghc 8.10.7

ghcup install cabal 3.4.0.0
ghcup set cabal 3.4.0.0
```

### Let's create a working directory to store the source-code and builds for the components

```bash
mkdir -p $HOME/cardano-src
cd $HOME/cardano-src
```

### Next, we will download, compile and install libsodium

```bash
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
```

### Once saved, we will then reload your shell profile to use the new variables. We can do that by typing 

```bash
source $HOME/.bashrc
```

### download, compile and install cardano-node and cardano-cli.

```bash
cd $HOME/cardano-src
```

output -->
/home/vscode/cardano-src/cardano-node

### Download the cardano-node repository

```bash
git clone https://github.com/input-output-hk/cardano-node.git
cd cardano-node
git fetch --all --recurse-submodules --tags
```

### Switch the repository to the latest tagged commit:

```bash
git checkout $(curl -s https://api.github.com/repos/input-output-hk/cardano-node/releases/latest | jq -r .tag_name)
```

### Configuring the build options

```bash
cabal configure --with-compiler=ghc-8.10.7
```

### We can now build the Haskell-based cardano-node to produce executable binaries.

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
