# [Run cardano-node]https://developers.cardano.org/docs/get-started/running-cardano)

```bash
vscode ➜ /workspaces/cardano-node-with-vscode-devcontainer (master ✗) $ cd relay/
$  cardano-node run \
--topology /workspaces/cardano-node-with-vscode-devcontainer/relay/testnet-topology.json \
--config /workspaces/cardano-node-with-vscode-devcontainer/relay/testnet-config.json \
--database-path /workspaces/cardano-node-with-vscode-devcontainer/relay/db \
--socket-path /workspaces/cardano-node-with-vscode-devcontainer/relay/db/node.socket \
--host-addr 172.17.0.2 \
--port 1337
```

## [Querying the Cardano Blockchain](https://developers.cardano.org/docs/querying-the-cardano-blockchain/)

cardano-cli and other Cardano software components need to know where the node socket file is located. 
The components read the shell environment variable CARDANO_NODE_SOCKET_PATH to find this.

### Set the environment variables

```bash
echo 'export CARDANO_NODE_SOCKET_PATH="~/db/node.socket"' >> ~/.bashrc
source ~/.bashrc 
echo $CARDANO_NODE_SOCKET_PATH
```

### Take care of the permission of the socket file (srwxr-xr-x)

```bash
vscode ➜ /workspaces/cardano-node-with-vscode-devcontainer/relay/db (master ✗) $ ls -la
total 24
drwxr-xr-x 5 vscode vscode 4096 Dec 13 09:52 .
drwxr-xr-x 3 vscode vscode 4096 Dec 12 16:32 ..
drwxr-xr-x 2 vscode vscode 4096 Dec 13 09:58 immutable
drwxr-xr-x 2 vscode vscode 4096 Dec 13 09:58 ledger
-rw-r--r-- 1 vscode vscode    0 Dec 13 09:52 lock
srwxr-xr-x 1 vscode vscode    0 Dec 13 09:52 node.socket
-rw-r--r-- 1 vscode vscode   10 Dec 13 09:52 protocolMagicId
drwxr-xr-x 2 vscode vscode 4096 Dec 13 09:58 volatile
```

### test querying the blockchain tip of our cardano-node (works only from the user vscode)

```bash
$ cardano-cli query tip --testnet-magic 1097911063
```

```bash
vscode ➜ ~ $ cardano-cli query tip --testnet-magic 1097911063
{
    "epoch": 0,
    "hash": "986dae80a38d19950927a6319a63901269548c9da2f92e67c49174aabfef29d9",
    "slot": 10565,
    "block": 9535,
    "era": "Byron",
    "syncProgress": "0.28"
}

vscode ➜ ~ $ cardano-cli query tip --testnet-magic 1097911063
{
    "epoch": 12,
    "hash": "ba8179108bcf6297f307b8320d070765b89a88ab06603d7ad07452dafd62ea56",
    "slot": 279467,
    "block": 278425,
    "era": "Byron",
    "syncProgress": "7.41"
}
```

vscode ➜ ~ $ cardano-cli query utxo --testnet-magic 1097911063 --address $SENDER