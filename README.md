# bep2-can
Repository to cover the listing of CAN on BinanceChain


## Setting up
Decide how to interact with BinanceChain:

|Type|Description|Server Requirements|
| --- | --- | --- |
|CLI (command line interface) | Useful for quickly interacting with BinanceChain. | Small server(1gb/25gb) |
|LightClient |Useful for deeper analysis 0f BinanceChain with only about 50mb of space required. |Small server (1gb/25gb) | 
|Full Node |Required to host the full chain. As as at date, testnet uses 6GB. |Medium server (16gb/320gb)| 


Create a new instance to host the Binance CLI client.

Chosen: **Ubuntu 16.04.6 x64 1gb/25gb DigitalOcean VPS**


Note the DEX and data-seed nodes:

https://testnet-dex.binance.org/api/v1/peers
```
https://testnet-dex.binance.org:443
https://data-seed-pre-0-s3.binance.org:443
https://data-seed-pre-1-s3.binance.org:443
https://data-seed-pre-2-s3.binance.org:443
https://seed-pre-s3.binance.org:443
```
*You should connect to one of the data-seed nodes*


Note testnet fees:

https://testnet-dex.binance.org/api/v1/fees


Mainnet Fees:

https://docs.binance.org/trading-spec.html#current-fees-table-on-mainnet



Note the ChainID to use: 

https://docs.binance.org/api-reference/cli.html#which-chain-id-to-use

Testnet: `Binance-Chain-Nile`

Mainnet: `Binance-Chain-Tigris`

## Run the CLI
Reference: https://docs.binance.org/api-reference/cli.html

Get the binary:
```
git clone https://github.com/binance-chain/node-binary.git
```

Get on to CLI (testnet v0.5.8)
```
cd node-binary/cli/testnet/0.5.8/linux
```

## Getting and Sending tBNB
Reference: https://docs.binance.org/transfer.html

Create three testnet accounts:
```
./tbnbcli keys add test_acc1
./tbnbcli keys add test_acc2
./tbnbcli keys add test_acc3
```

Get tBNB tokens (5 x 200BNB required): 
https://www.binance.com/en/dex/testnet/address

Query your own balance:
```
./tbnbcli account tbnb1xxxxxxx --chain-id Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:443 --indent
```

Send enough of your test BNB to a single account:
```
./tbnbcli send --chain-id=Binance-Chain-Nile --from=test_acc2 --amount=10000000000:BNB --to=tbnb1xxxx
```
*note: 100000000:BNB is equal to 1 BNB (8 decimal places)*

## Creating Tokens
Reference: https://docs.binance.org/tokens.html

Issue a test Token (burns 200BNB):
```
./tbnbcli token issue --token-name “testTKN” --total-supply 10000000000000000 --symbol tTKN --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --trust-node --from test_acc1
```

Practice burning some:
```
./tbnbcli token burn --amount 33300000000 --symbol XXX-XXX --from test_acc1 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --trust-node
```

Practise freezing some:
```
./tbnbcli token freeze --amount 33300000000 --symbol XXX-XXX --from test_acc1 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --trust-node
```

## Submit a listing proposal
Reference: https://docs.binance.org/governance.html

Requires: 800BNB

Create a listing proposal. 
```
./tbnbcli gov submit-list-proposal --from test_acc1 --deposit 10000000000:BNB --base-asset-symbol XXX-XXX --quote-asset-symbol BNB --init-price 1000000 --title "list XXX-XXX/BNB" --description "list XXX-XXX/BNB" --expire-time 1556586654 --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json
```

Note, `expire-time` is in unix date form. To convert date to unix epoch:
```
date -d "2019-05-01" +"%s"
```

Once voted by Binance validators, then submit a listing transaction:
```
./tbnbcli dex list -s XXX-XXX --quote-asset-symbol BNB --from test_acc1 —init-price 100000000 --proposal-id XXX --chain-id=Binance-Chain-Nile --node=data-seed-pre-2-s1.binance.org:80 --json
```
https://docs.binance.org/list.html
