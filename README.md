# regtest-manager
*A quick and dirty solution to start a (Lightning) regtest*  
  
## Rationale
  
Setup a regtest is both cumbersome (/repetitive) and pretty useful for a lot of Bitcoin / Lightning Network applications (e.g. plugins). I needed that automation. So I did it and published it here.  
This does not intend to be robust (or clean :-)), just an utility (source it in your bashrc). For a more complete solution, check out [lnet](https://github.com/cdecker/lnet).  
  
## Usage
  
Quick setup:
```
source regtest_manager
start_regtest 3 && fund_regtest # A minute later you have a fully set up LN regtest
```
  
By sourcing the `regtest_manager` script you can use 2 functions to start a test network of any number of nodes.  
```bash
source regtest_manager
start_regtest # Default is 2 nodes, otherwise the number can be specified as an argument
```
==>
```bash
$ start_regtest 3
Started bitcoind n.1 with P2P port 10001, RPC port 9001 and datadir /home/darosior/projects/pylightning-qt/regtest/bcdir1
Started lightningd on top of it with directory /home/darosior/projects/pylightning-qt/regtest/lndir1

Started bitcoind n.2 with P2P port 10002, RPC port 9002 and datadir /home/darosior/projects/pylightning-qt/regtest/bcdir2
Started lightningd on top of it with directory /home/darosior/projects/pylightning-qt/regtest/lndir2

Started bitcoind n.3 with P2P port 10003, RPC port 9003 and datadir /home/darosior/projects/pylightning-qt/regtest/bcdir3
Started lightningd on top of it with directory /home/darosior/projects/pylightning-qt/regtest/lndir3


Started 3 pairs of bitcoind and lightningd nodes with rpc user "test" and pass "test".
You can access them using aliases created for each one : bcregi and lnregi with i the node number for bitcoin-cli and lightning cli.
For example you can try with "bcreg2 getblockchaininfo" and "lnreg2 getinfo"
```
To access a node, use lnreg_i_ or bcreg_i_ with i the node number. For example:
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
You can fund a channel between all nodes (like l1 --> l2 --> l3) with:
```bash
$ fund_regtest
Getting some bitcoins on both 'bitcoind' and 'lightningd' for each node
Funding a channel between node 1 and node 2 :
Waiting for lightningd to be aware of its bitcoins
{
   "tx" : "020000000001012df2a1c4b192a2fc741f840e076b74c1506b4c0dc6f7ad8aac00c1a1b10fa93c0100000000ffffffff02a08601000000000022002033327d832c3d4a234c605c9ede6dbcb399440d6ae68bcb310be244a31a3839faa942993b00000000160014028d6e1310ab72afb6102859c0b289f686a7264602483045022100844bb79d9b007713cc7ea27ae78f9cef31829c892edcc5737311516c08142a3502207c329763740644128c0125e42421b6fa7e5e557663676af88a151eb036e95a350121026b1bf2a838e6e262d280c70007e0d1729d116f143053c35a3fffe174d9cc01a900000000",
   "txid" : "6ef07a54f59131f31ab8ce2965dd33b316a20d30a7c114973fd959eac5947a09",
   "channel_id" : "097a94c5ea59d93f9714c1a7300da216b333dd6529ceb81af33191f5547af06e"
}

Funding a channel between node 2 and node 3 :
Waiting for lightningd to be aware of its bitcoins
{
   "tx" : "0200000000010111276268ef570cd98c32fa81985d746351535b94010959e8fbb8c77cb3689d690100000000ffffffff02a0860100000000002200201eef1f82e3f08951a7ed585ded129fbee0b56fae0663294e90ae824813245f35a942993b00000000160014d67ee0343bc0189c63b2d8f2e0cd63cb8683d8ea02483045022100e0875e1d1770c9a7c439b4bbe1065ed7e5ab9b74e06c6318106febc8f7f23ec602202f6da17be93f4b83bd2acd876e9b3647d3088be3aaa3a2b14de5db07aacb6631012102a5be7ce4872a3c50cb8ed05e59bc42bd54bf796c28754984fc9c4fbfc0823f8b00000000",
   "txid" : "a7719bc2a6fc8afb604c79083b893e3cba3ab7528d64db5ea74eeb5e81c3ac72",
   "channel_id" : "72acc3815eeb4ea75edb648d52b73aba3c3e893b08794c60fb8afca6c29b71a7"
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
