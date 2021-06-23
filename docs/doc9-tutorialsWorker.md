---
id: PolkaDexWorkerHowTo
title: How to start & talk to the polkadextee-worker
sidebar_label: Tutorials
---

This is currently just a Knowhow dump, not a full fledged tutorial.

### Worker Start Up
Once you have started the Polkadex node and the openfinex server, you can finally start the worker:
```bash
cd polkadexTEE-worker/bin
# ensure that you're on the develop branch (just like the node)
# run the worker with: 
# -P <WORKERPORT> the user client can talk to
# -p <NODEPORT> the node is listening on
# -F <OPENFINEXPORT+PATH> the openfinex port and path
# -f <OPENFINEXADDRESS> the openfinex server address
# if one of the above is not specified, a default value will be taken instead.
 ./substratee-worker -P 2094 -p 9994 -F 8001/api/v2/ws -f 127.0.0.1 run --skip-ra
```

### Talk to the worker (with --direct) and node with the Client
```bash
### Indiret calls (to node):
cd polkadexTEE-worker/bin
# register Account
./substratee-client -p 9994 -P 2094 register-account //Alice
# deposit funds to offchain account
./substratee-client -p 9994 -P 2094 deposit --accountid=//Alice --tokenid=polkadex --quantity=10000
# withdraw funds from offchain account
 ./substratee-client -p 9994 -P 2094 withdraw --accountid=//Bob --tokenid=dot --quantity=1000
### Direct calls (to worker):
# place order
./substratee-client -p 9994 -P 2094 trusted place_order --accountid=//Alice --marketbase=polkadex --marketquote=dot \
   --markettype=trusted --ordertype=limit --price=100 --orderside=bid --quantity=100 --mrenclave==6J3VHscuA9WmiQHf84CK1CHu87GkNcwBAyJ6pPjaCvrv --direct
# get balance
./substratee-client -p 9994 -P 2094 get_balance --accountid=//Alice --tokenid=usd --mrenclave=9GSUY4WM3Yim5337AKweJN5Kb9K4yHjFmToLpNccwu7L --direct
# cancel order
./substratee-client -p 9994 -P 2094 trusted cancel_order --accountid=//Alice --proxyaccountid=//AliceIncognitoProxy \
    --orderid=oijef03jaf --mrenclave 9GSUY4WM3Yim5337AKweJN5Kb9K4yHjFmToLpNccwu7L --direct
#  withdraw funds
./substratee-client -p 9994 -P 2094 trusted withdraw --accountid=//Alice --proxyaccountid=//AliceProxy \
    --tokenid=dot --quantity=293 --mrenclave 9GSUY4WM3Yim5337AKweJN5Kb9K4yHjFmToLpNccwu7L --direct
```
The mreclace is printed at every worker start up, just get it from there.