# Commands to run on your EnigmaChain node

## Check the total supply

```bash
enigmacli q supply total
```

## Check the balance of the community pool
```bash
enigmacli q distribution community-pool
```

## Check your validator address.
```bash
enigmacli keys show <keyalias> --bech=val -a
```

## Follow node logs to debug (exit with Ctrl-C)
```bash
journalctl -f -u enigma-node
```

## Query account
```bash
enigmacli q account $(enigmacli keys show -a <key_alias>)
```

## Staking more tokens

(remember 1 SCRT = 1,000,000 uSCRT)

```bash
enigmacli tx staking delegate $(enigmacli keys show <key-alias> --bech=val -a) <amount>uscrt --from <key-alias>
```

## Renaming your moniker

```bash
enigmacli tx staking edit-validator --moniker <new-moniker> --from <key-alias>
```

## Seeing your rewards from being a validator

```bash
enigmacli q distribution rewards $(enigmacli keys show -a <key-alias>)
```

## Seeing your commissions from your delegators

```bash
enigmacli q distribution commission $(enigmacli keys show -a <key-alias> --bech=val)
```

## Withdrawing rewards

```bash
enigmacli tx distribution withdraw-rewards $(enigmacli keys show --bech=val -a <key-alias>) --from <key-alias>
```

## Withdrawing rewards+commissions

```bash
enigmacli tx distribution withdraw-rewards $(enigmacli keys show --bech=val -a <key-alias>) --from <key-alias> --commission
```