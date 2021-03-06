# How to join the Enigma Blockhain as a mainnet validator

### 1. Run a new full node

[Setup a fullnode](https://secretnodes.org/#/tutorials/deploy-enigma-fullnode) on a new machine.

Be sure to wait for the node to be fully syncd before continuing. This may take several hours. In the future there will be a quicksync service available for use.

### 2. Generate a new key pair for yourself (change `<key-alias>` with any word of your choice, this is just for your internal/personal reference):

```bash
secretcli keys add <key-alias>
```

**:warning:Note:warning:: Backup the mnemonics!**
**:warning:Note:warning:: Please make sure you also [backup your validator](https://secretnodes.org/#/tutorials/backup-a-validator)**

**Note**: If you already have a key you can import it with the bip39 mnemonic with `secretcli keys add <key-alias> --recover` or with `secretcli keys export` (exports to `stderr`!!) & `secretcli keys import`.

### 3. Output your node address:

```bash
secretcli keys show <key-alias> -a
```

### 4. Transfer tokens to the address displayed above.

### 5. Check that you have the requested tokens:

```bash
secretcli q account $(secretcli keys show -a <key_alias>)
```

If you get the following message, it means that you have no tokens yet:

```bash
ERROR: unknown address: account enigmaxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx does not exist
```

### 6. Join the network as a new validator: replace `<MONIKER>` with your own from step 3 above, and adjust the amount you want to stake

(remember 1 SCRT = 1,000,000 uSCRT, and so the command below stakes 100k SCRT).

```bash
secretcli tx staking create-validator \
  --amount=100000000000uscrt \
  --pubkey=$(secretd tendermint show-validator) \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --gas=200000 \
  --gas-prices="0.025uscrt" \
  --moniker=<MONIKER> \
  --from=<key-alias>
```

### 7. Check that you have been added as a validator:

```bash
secretcli q staking validators
```

If the above is too verbose, just run: `secretcli q staking validators | grep moniker`. You should see your moniker listed.

## Staking more tokens

(remember 1 SCRT = 1,000,000 uSCRT)

In order to stake more tokens beyond those in the initial transaction, run:

```bash
secretcli tx staking delegate $(secretcli keys show <key-alias> --bech=val -a) <amount>uscrt --from <key-alias>
```

## Renaming your moniker

```bash
secretcli tx staking edit-validator --moniker <new-moniker> --from <key-alias>
```

## Seeing your rewards from being a validator

```bash
secretcli q distribution rewards $(secretcli keys show -a <key-alias>)
```

## Seeing your commissions from your delegators

```bash
secretcli q distribution commission $(secretcli keys show -a <key-alias> --bech=val)
```

## Withdrawing rewards

```bash
secretcli tx distribution withdraw-rewards $(secretcli keys show --bech=val -a <key-alias>) --from <key-alias>
```

## Withdrawing rewards+commissions

```bash
secretcli tx distribution withdraw-rewards $(secretcli keys show --bech=val -a <key-alias>) --from <key-alias> --commission
```

## Removing your validator

Currently deleting a validator is not possible. If you redelegate or unbond your self-delegations then your validator will become offline and all your delegators will start to unbond.

## Unjailing your validator

Using the following command you can unjail your node.

```bash
secretcli tx slashing unjail --from =<key-alias>
```
[Original Source](https://github.com/enigmampc/EnigmaBlockchain/blob/master/docs/validators-and-full-nodes/join-validator-mainnet.md)