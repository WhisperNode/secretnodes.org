# How to backup a validator

It is **CRUCIAL** to backup your validator's private key. It's the only way to restore your validator in an event of a disaster.

The validator private key is a Tendermint Key - a unique key used to sign consensus votes. It is generated when the node is created by `enigmad init`.

To see the associated public key:

```bash
enigmad tendermint show-validator
```

To see the associated bech32 address:

```bash
enigmad tendermint show-address
```

If you are using the software sign (which is the default signing method of tendermint), your Tendermint Key is located in `~/.enigmad/config/priv_validator_key.json`.

The easiest way is to backup the whole config folder.

Or you can use hardware to manage your Tendermint Key much more safely, such as [YubiHSM2](https://developers.yubico.com/YubiHSM2/)

[Original Source](https://github.com/enigmampc/enigmachain/blob/master/docs/validators-and-full-nodes/backup-a-validator.md)