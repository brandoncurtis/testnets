# Testnets

This repository contains directories with the files required to join an identically named testnet.

See the links below for documentation on joining a public - or creating a local - testnet.

* [deploy tendermint testnets](http://tendermint.readthedocs.io/en/master/deploy-testnets.html)
* [join ethermint testnet](http://ethermint.readthedocs.io/en/develop/testnets/venus.html)
* [join/create gaia testnet](http://cosmos-sdk.readthedocs.io/en/develop/staking/intro.html)


## Brandon Curtis' Testnet Notes for New Folks

Want to join a testnet?  The currently-active one is **unofficial-102**.

generate new keys:

`gaia client keys new <yourname>`

make sure they look good:

`gaia client keys list`

place the provided genesis.json and config.toml into a home directory for this chain:

`mkdir ~/.cosmos-unofficial-102`
`cp * ~/.cosmos-unofficial-102/`

Now start the node:

`gaia node start --home ~/.cosmos-unofficial-102`

If you take a look inside that folder, you'll see that it generated a validator key (priv_validator.json) and created files to manage peering (addrbook.json) and a folder for chain data.

If you already have a validator key that you'd like to use, replace priv_validator.json with your validator key.
Trim your existing validator key's json down to just the address, pub_key, and priv_key, removing any reference to previous heights, rounds, steps, and signatures.

To use the Gaia CLI utilities, you need to initialize the client to "establish a root of trust":

`gaia client init --chain-id unofficial-102 --node=tcp://0.0.0.0:46657`

To check if it worked, try querying the client for the node's current status:

`gaia client rpc status`

Now try querying the balance of your address:

`gaia client query account 3A75D80740B58E7C769CC374BBD014E87337F0E9`

If you have participated in a Cosmos testnet before, I may have collected your old address and allocated it some coins in the genesis block.  If not, hit us up on Riot Chat (Matrix) and we can send you some coins.

If you have coins, you can send them to other accounts:

`gaia client tx send --name {default,your_account} --to A1457DF58614286DC70EBCBA50FD50B37262E4B5 --amount 1fermion`

Check out `gaia client tx --help` for more stuff you can do.

Now try querying your a validator candidate to see its voting power. Try the validator that I launched the network with:

`gaia client query candidate --pubkey 98683F1418228F4D5FEEAFE98E77BEF9DF19D81B9794CC786D40B533D67DA21E`


### Setting Up a Validator

If you are joining the network with a computer that will be turned on and connected to the internet without interruption, you may wish to try setting up a validator.  If you can't guarantee uptime, don't set up a validator—if more than 1/3rd of the total validator power leaves the network, the network will halt for everyone!

First you have to declare your validator node for candidacy:

`gaia client tx declare-candidacy --name {default,youraccountname} --amount 1fermion --pubkey {pubkey_from_priv_validator} --moniker {yourname} --details "info about your validator"`

Check to see if it worked; your validator pubkey should be added to the list of candidates:

`gaia client query candidates`

Once a validator node has been declared for candidacy, you can delegate additional stake to increase this validator's voting power:

`gaia client tx delegate --name brandon --amount 1fermion --pubkey {pubkey_from_priv_validator}`

For instance, try binding an additional fermion to the initial validator I used to launch the network:

`gaia client tx delegate --name brandon --amount 1fermion --pubkey 98683F1418228F4D5FEEAFE98E77BEF9DF19D81B9794CC786D40B533D67DA21E`


### Doing Other Stuff

It's a good idea to back up your account key recovery phrase so that you can recover it with `gaia client keys recover <yourname>` if you need to.

If you set up a validator, you should back up priv_validator.json too.

Sometimes a node will get stuck; resettting it usually fixes it.

Worse-case, try deleting the data directory and starting over again.


