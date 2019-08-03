# regtest-manager
*A quick and dirty solution to start a (Lightning) regtest*  
  
## Rationale
  
Setup a regtest is both cumbersome (/repetitive) and pretty useful for a lot of Bitcoin / Lightning Network applications (e.g. plugins). I needed that automation. So I did it and published it here.  
This does not intend to be robust (or clean :-)), just an utility (source it in your bashrc). For a more complete solution, check out [lnet](https://github.com/cdecker/lnet).  
  
## Usage
  
By sourcing the `regtest_manager` script you can use 2 functions to start a test network of any number of nodes.  
```bash
source regtest_manager
start_regtest # Default is 2 nodes, otherwise the number can be specified as an argument
```
==>
```bash
$ start_regtest 3
Started bitcoind n.1 with P2P port 10001, RPC port 9001 and datadir /home/darosior/test/regtest/bcdir1
Started lightningd on top of it with directory /home/darosior/test/regtest/lndir1

Started bitcoind n.2 with P2P port 10002, RPC port 9002 and datadir /home/darosior/test/regtest/bcdir2
Started lightningd on top of it with directory /home/darosior/test/regtest/lndir2


Started 2 pairs of bitcoind and lightningd nodes with rpc user "test" and pass "test".
You can access them using aliases created for each one : bcregi and lnregi with i the node number for bitcoin-cli and lightning cli.
For example you can try with "bcreg2 getblockchaininfo" and "lnreg2 getinfo"
```
To access a node, use lnreg__i__ or bcreg__i__ with i the node number. For example:
```bash
$ lnreg2 getinfo
{
   "id" : "03b1762af584f80a1b644f029d7f13c1f58e4c53a54e0e0a944454813f76a86b2f",
   "alias" : "testnode2",
   "color" : "03b176",
   "num_peers" : 0,
   "num_pending_channels" : 0,
   "num_active_channels" : 0,
   "num_inactive_channels" : 0,
   "address" : [],
   "binding" : [
      {
         "type" : "ipv4",
         "address" : "127.0.0.1",
         "port" : 11002
      }
   ],
   "version" : "v0.7.1-217-g913a1a9",
   "blockheight" : 0,
   "network" : "regtest",
   "msatoshi_fees_collected" : 0,
   "fees_collected_msat" : "0msat"
}
```
Finally, you can stop them one by one but also stop them all via (which deletes also data directories):
```bash
$ stop_regtest 
lightningd can take some time to stop. It will be fixed in v0.7.2
lightningd n.1 stopped
bitcoind n.1 stopped
lightningd n.2 stopped
bitcoind n.2 stopped
```
