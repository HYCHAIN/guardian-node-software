# HYCHAIN Guardian Node Software

This repository contains the compiled command line interface binaries for running a guardian node on Windows, Mac and Linux operating systems.

Use this repository if you intend to run your guardian node on your own device. Alternatively, if you'd prefer to use a hosted service to quickly get a node up and running, [you can use our node hosting partners NodeOps, here](https://nodeops.xyz/).

By operating the HYCHAIN Guardian Node Software, you [agree to our terms of service](https://github.com/HYCHAIN/node-tos).

## Quickstart

A guardian submit assertions for node keys that they either own or have been approved to submit assertions for.

Command to run guardian software. This is will validate and submit assertions for all node keys that are approved for, or owned by the <guardian-private-key> address.

```sh
./cli/{your-os}/guardian-cli-{your-os} guardian run <guardian-private-key> --loop-interval-ms 3600000
```
​
Command to run to claim rewards for a list of node keys. Signer private key can be for any wallet with sufficient native token (TOPIA) to pay for gas required to claim rewards.

```sh
./cli/{your-os}/guardian-cli-{your-os} guardian claim-multiple-rewards <node-key-ids> <signer-private-key>
```

Please note, the address associated with the <guardian-private-key> must maintain a $TOPIA balance on HYCHAIN in order to properly submit assertions or claims.

## Challenge Architecture / Flow Overview

1. A challenge is produced every time a batch from the Rollup is submitted to Ethereum. This is done by a challenger service hosted by Caldera
2. Each Node Key will submit assertions to a subset of challenges with an average of 1 assertion per day per node key. Whether a node key is eligible to submit an assertion to a challenge is determined on chain.
3. Each challenge will have an expiry period of 1 day. This means a guardian will need to run the guardian software successfully at least once a day, or keep the guardian node running constantly with `--loop-interval-ms`
4. Once a challenge expires, the challenger service will verify all assertions made and revoke any node keys who submitted invalid assertions.

## Expanded Usage Details

For the main workflow of submitting assertions for user node keys:

IMPORTANT: Make sure to execute the binary from the directory it is contained in.

```sh
./cli/{your-os}/guardian-cli-{your-os} guardian run <guardian-private-key>
```

This will start the guardian and submit assertions for all node keys owned / approved for the private key address for currently open challenges. It will then end and exit the program. Additionally the --node-key-allowlist <node-key-ids>  can be used to only submit assertions for a list of keys and --loop-interval-ms <loop-interval-ms> can be used to keep the guardian running forever (with a time delay between checking). If this option is used, we reccomend an interval of an hour since new challenges are made on average once an hour.

To claim rewards:

```sh
./cli/{your-os}/guardian-cli-{your-os} guardian claim-rewards --owned-keys --approved-keys
```

For other usage, refer to the cli help manual

```sh
$ ./cli help guardian

Usage: guardian-cli guardian [options] [command]

Guardian Node related commands

Options:
  -h, --help                                          display help for command

Commands:
  run [options] <guardian-private-key>                Run the Guardian service
  reward-to-claim <node-key-ids>                      Get rewards to claim for multiple Node Keys
  node-keys-approved-for-guardian <guardian-address>  Get node keys that a guardian is approved for via delegate.xyz
  claim-rewards [options] <signer-private-key>        Claim rewards for a list of Node Keys
  total-claimed-rewards-for-node-keys <node-key-ids>  Get total claimed rewards for a list of Node Keys
  owned-node-keys <address>                           Get node keys that a public address owns
  help [command]                                      display help for command
```

Can run help <command> for more details on each command. Example:

```sh
$ ./cli/linux/guardian-cli-linux guardian help reward-to-claim

Usage: guardian-cli guardian reward-to-claim [options] <node-key-ids>

Get rewards to claim for multiple Node Keys

Arguments:
  node-key-ids  The IDs of the Node Keys

Options:
  -h, --help    display help for command
```

