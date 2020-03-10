# How to deploy a full node

This document details how to join the Enigma Blockchain `mainnet` as a validator.


>! Note : If you need assistance in preparing a host machine to run as a Full Node, please see this guide first [Prepare your host machine](tutorials/prepare-your-host-machine.md "Prepare your host machine")

[![asciicast](https://asciinema.org/a/29aZHQ9gIApaPQ2Wi325DmpBf.svg)](https://asciinema.org/a/29aZHQ9gIApaPQ2Wi325DmpBf)

>! Note : Video may differ slightly.

## Requirements

>! The OS Reccomended by secretnodes.org is Ubuntu Server 18.04 LTS
- Ubuntu/Debian host (with ZFS or LVM to be able to add more storage easily)
- A public IP address
- Open ports `TCP 26656 & 26657` _Note: If you're behind a router or firewall then you'll need to port forward on the network device._
- Reading https://docs.tendermint.com/master/tendermint-core/running-in-production.html

### 1. Download the [Enigma Blockchain package installer](https://github.com/enigmampc/EnigmaBlockchain/releases/download/v0.0.2/enigmachain_0.0.2_amd64.deb) (Debian/Ubuntu):

```bash
wget https://github.com/enigmampc/EnigmaBlockchain/releases/download/v0.0.2/enigmachain_0.0.2_amd64.deb
```

([How to verify releases](/docs/verify-releases.md))

### 2. Install the package:

```bash
sudo dpkg -i enigmachain_0.0.2_amd64.deb
```

### 3. Initialize your installation of the Enigma Blockchain. Choose a **moniker** for yourself that will be public, and replace `<MONIKER>` with your moniker below

```bash
enigmad init <MONIKER> --chain-id enigma-1
```

### 4. Download a copy of the Genesis Block file: `genesis.json`

```bash
wget -O ~/.enigmad/config/genesis.json "https://raw.githubusercontent.com/enigmampc/EnigmaBlockchain/master/enigma-1-genesis.json"
```

### 5. Validate the checksum for the `genesis.json` file you have just downloaded in the previous step:

```
echo "86cd9864f5b8e7f540c5edd3954372df94bd23de62e06d5c33a84bd5f3d29114 $HOME/.enigmad/config/genesis.json" | sha256sum --check
```

### 6. Validate that the `genesis.json` is a valid genesis file:

```
enigmad validate-genesis
```

### 7. Add `bootstrap.mainnet.enigma.co` as a persistent peer in your configuration file.

If you are curious, you can query the RPC endpoint on that node http://bootstrap.mainnet.enigma.co:26657/ (please note that the RPC port `26657` is different from the P2P port `26656` below)

```
perl -i -pe 's/persistent_peers = ""/persistent_peers = "201cff36d13c6352acfc4a373b60e83211cd3102\@bootstrap.mainnet.enigma.co:26656"/' ~/.enigmad/config/config.toml
```

### 8. Listen for incoming RPC requests so that light nodes can connect to you:

```bash
perl -i -pe 's/laddr = .+?26657"/laddr = "tcp:\/\/0.0.0.0:26657"/' ~/.enigmad/config/config.toml
```

### 9. Enable `enigma-node` as a system service:

```
sudo systemctl enable enigma-node
```

### 10. Start `enigma-node` as a system service:

```
sudo systemctl start enigma-node
```

### 11. If everything above worked correctly, the following command will show your node streaming blocks (this is for debugging purposes only, kill this command anytime with Ctrl-C):

```bash
journalctl -f -u enigma-node
```

```
-- Logs begin at Mon 2020-02-10 16:41:59 UTC. --
Feb 10 21:18:34 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:34.307] Executed block                               module=state height=2629 validTxs=0 invalidTxs=0
Feb 10 21:18:34 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:34.317] Committed state                              module=state height=2629 txs=0 appHash=34BC6CF2A11504A43607D8EBB2785ED5B20EAB4221B256CA1D32837EBC4B53C5
Feb 10 21:18:39 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:39.382] Executed block                               module=state height=2630 validTxs=0 invalidTxs=0
Feb 10 21:18:39 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:39.392] Committed state                              module=state height=2630 txs=0 appHash=17114C79DFAAB82BB2A2B67B63850864A81A048DBADC94291EB626F584A798EA
Feb 10 21:18:44 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:44.458] Executed block                               module=state height=2631 validTxs=0 invalidTxs=0
Feb 10 21:18:44 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:44.468] Committed state                              module=state height=2631 txs=0 appHash=D2472874A63CE166615E5E2FDFB4006ADBAD5B49C57C6B0309F7933CACC24B10
Feb 10 21:18:49 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:49.532] Executed block                               module=state height=2632 validTxs=0 invalidTxs=0
Feb 10 21:18:49 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:49.543] Committed state                              module=state height=2632 txs=0 appHash=A14A58E80FB24115DD41E6D787667F2FBBE003895D1B79334A240F52FCBD97F2
Feb 10 21:18:54 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:54.613] Executed block                               module=state height=2633 validTxs=0 invalidTxs=0
Feb 10 21:18:54 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:54.623] Committed state                              module=state height=2633 txs=0 appHash=C00112BB0D9E6812CEB4EFF07D2205D86FCF1FD68DFAB37829A64F68B5E3B192
Feb 10 21:18:59 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:59.685] Executed block                               module=state height=2634 validTxs=0 invalidTxs=0
Feb 10 21:18:59 ip-172-31-41-58 enigmad[8814]: I[2020-02-10|21:18:59.695] Committed state                              module=state height=2634 txs=0 appHash=1F371F3B26B37A2173563CC928833162DDB753D00EC2BCE5EDC088F921AD0D80
^C
```

You are now a full node. :tada:

### 12. Add the following configuration settings (some of these avoid having to type some flags all the time):

```bash
enigmacli config chain-id enigma-1
```

```bash
enigmacli config output json
```

```bash
enigmacli config indent true
```

```bash
enigmacli config trust-node true # true if you trust the full-node you are connecting to, false otherwise
```

### 13. Get your node ID with:

```bash
enigmacli status | awk -F \" '/"id"/{print $4}'
```

And publish yourself as a node with this ID:

```
<your-node-id>@<your-public-ip>:26656
```

So if someone wants to add you as a peer, have them add the above address to their `persistent_peers` in their `~/.enigmad/config/config.toml`.  
And if someone wants to use you from their `enigmacli` then have them run:

```bash
enigmacli config chain-id enigma-1
```

```bash
enigmacli config output json
```

```bash
enigmacli config indent true
```

```bash
enigmacli config trust-node false
```

```bash
enigmacli config node tcp://<your-public-ip>:26657
```

[Original Source](https://github.com/enigmampc/enigmablockchainblob/master/docs/validators-and-full-nodes/run-full-node-mainnet.md)
