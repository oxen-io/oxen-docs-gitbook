# Daemon 📞 RPC calls

### Introduction

This is a list of the oxend daemon RPC calls, their inputs and outputs, and examples of each.

Many RPC calls use the daemon's JSON RPC interface while others use their own interfaces, as demonstrated below.

Note: "atomic units" refer to the smallest fraction of 1 $OXEN according to the oxend implementation. **1 $OXEN = 1e12 atomic units.**

#### JSON RPC Methods:

* [get\_block\_count](daemon-rpc-calls.md#get_block_count)
* [on\_get\_block\_hash](daemon-rpc-calls.md#on_get_block_hash)
* [get\_block\_template](daemon-rpc-calls.md#get_block_template)
* [submit\_block](daemon-rpc-calls.md#submit_block)
* [get\_last\_block\_header](daemon-rpc-calls.md#get_last_block_header)
* [get\_block\_header\_by\_hash](daemon-rpc-calls.md#get_block_header_by_hash)
* [get\_block\_header\_by\_height](daemon-rpc-calls.md#get_block_header_by_height)
* [get\_block\_headers\_range](daemon-rpc-calls.md#get_block_headers_range)
* [get\_block](daemon-rpc-calls.md#get_block)
* [get\_connections](daemon-rpc-calls.md#get_connections)
* [get\_info](daemon-rpc-calls.md#get_info)
* [hard\_fork\_info](daemon-rpc-calls.md#hard_fork_info)
* [set\_bans](daemon-rpc-calls.md#set_bans)
* [get\_bans](daemon-rpc-calls.md#get_bans)
* [flush\_txpool](daemon-rpc-calls.md#flush_txpool)
* [get\_output\_histogram](daemon-rpc-calls.md#get_output_histogram)
* [get\_version](daemon-rpc-calls.md#get_version)
* [get\_coinbase\_tx\_sum](daemon-rpc-calls.md#get_coinbase_tx_sum)
* [get\_fee\_estimate](daemon-rpc-calls.md#get_fee_estimate)
* [get\_alternate\_chains](daemon-rpc-calls.md#get_alternate_chains)
* [relay\_tx](daemon-rpc-calls.md#relay_tx)
* [get\_txpool\_backlog](daemon-rpc-calls.md#get_txpool_backlog)
* [get\_output\_distribution](daemon-rpc-calls.md#get_output_distribution)

#### Other RPC Methods:

* [/get\_height](daemon-rpc-calls.md#get_height)
* [/get\_blocks.bin](daemon-rpc-calls.md#get_blocks-bin)
* [/get\_blocks\_by\_height.bin](daemon-rpc-calls.md#get_blocks_by_height-bin)
* [/get\_hashes.bin](daemon-rpc-calls.md#get_hashes-bin)
* [/get\_o\_indexes.bin](daemon-rpc-calls.md#get_o_indexes-bin)
* [/get\_outs.bin](daemon-rpc-calls.md#get_outs-bin)
* [/get\_transactions](daemon-rpc-calls.md#get_transactions)
* [/get\_alt\_blocks\_hashes](daemon-rpc-calls.md#get_alt_blocks_hashes)
* [/is\_key\_image\_spent](daemon-rpc-calls.md#is_key_image_spent)
* [/send\_raw\_transaction](daemon-rpc-calls.md#send_raw_transaction)
* [/save\_bc](daemon-rpc-calls.md#save_bc)
* [/get\_peer\_list](daemon-rpc-calls.md#get_peer_list)
* [/set\_log\_hash\_rate](daemon-rpc-calls.md#set_log_hash_rate)
* [/set\_log\_level](daemon-rpc-calls.md#set_log_level)
* [/set\_log\_categories](daemon-rpc-calls.md#set_log_categories)
* [/get\_transaction\_pool](daemon-rpc-calls.md#get_transaction_pool)
* [/get\_transaction\_pool\_hashes.bin](daemon-rpc-calls.md#get_transaction_pool_hashes-bin)
* [/get\_transaction\_pool\_stats](daemon-rpc-calls.md#get_transaction_pool_stats)
* [/stop\_daemon](daemon-rpc-calls.md#stop_daemon)
* [/get\_info \(not JSON\)](daemon-rpc-calls.md#get_info-not-json)
* [/get\_limit](daemon-rpc-calls.md#get_limit)
* [/set\_limit](daemon-rpc-calls.md#set_limit)
* [/out\_peers](daemon-rpc-calls.md#out_peers)
* [/in\_peers](daemon-rpc-calls.md#in_peers)
* [/start\_save\_graph](daemon-rpc-calls.md#start_save_graph)
* [/stop\_save\_graph](daemon-rpc-calls.md#stop_save_graph)
* [/get\_outs](daemon-rpc-calls.md#get_outs)
* [/update](daemon-rpc-calls.md#update)

### JSON RPC Methods

The majority of oxend RPC calls use the daemon's `json_rpc` interface to request various bits of information. These methods all follow a similar structure, for example:

```text
IP=127.0.0.1
PORT=18081
METHOD='get_block_header_by_height'
ALIAS='getblockheaderbyheight'
PARAMS='{"height":912345}'
curl \
    -X POST http://$IP:$PORT/json_rpc \
    -d '{"jsonrpc":"2.0","id":"0","method":"'$METHOD'","params":'$PARAMS'}' \
    -H 'Content-Type: application/json'
```

Some methods include parameters, while others do not. Examples of each JSON RPC method follow.

#### **get\_block\_count**

Look up how many blocks are in the longest chain known to the node.

Alias: _getblockcount_.

Inputs: _None_.

Outputs:

* _count_  - unsigned int; Number of blocks in longest chain seen by the node.
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_count"}' -H 'Content-Type: application/json'  

{  
  "id": "0",  
  "jsonrpc": "2.0",  
  "result": {  
    "count": 993163,  
    "status": "OK"  
  }  
}
```

#### **on\_get\_block\_hash**

Look up a block's hash by its height.

Alias: _on\_getblockhash_.

Inputs:

* block height \(int array of length 1\)

Outputs:

* block hash \(string\)

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"on_get_block_hash","params":[912345]}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6"
}
```

#### **get\_block\_template**

Get a block template on which mining a new block.

Alias: _getblocktemplate_.

Inputs:

* _wallet\_address_  - string; Address of wallet to receive coinbase transactions if block is successfully mined.
* _reserve\_size_  - unsigned int; Reserve size.

Outputs:

* _blocktemplate\_blob_  - string; Blob on which to try to mine a new block.
* _blockhashing\_blob_  - string; Blob on which to try to find a valid nonce.
* _difficulty_  - unsigned int; Difficulty of next block.
* _expected\_reward_  - unsigned int; Coinbase reward expected to be received if block is successfully mined.
* _height_  - unsigned int; Height on which to mine.
* _prev\_hash_  - string; Hash of the most recent block on which to mine the next block.
* _reserved\_offset_  - unsigned int; Reserved offset.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_template","params":{"wallet_address":"44GBHzv6ZyQdJkjqZje6KLZ3xSyN1hBSFAnLP6EAqJtCRVzMzZmeXTC2AHKDS9aEDTRKmo6a6o9r9j86pYfhCWDkKjbtcns","reserve_size":60}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "blockhashing_blob": "070786a498d705f8dc58791266179087907a2ff4cd883615216749b97d2f12173171c725a6f84a00000000fc751ea4a94c2f840751eaa36138eee66dda15ef554e7d6594395827994e31da10",
    "blocktemplate_blob": "070786a498d705f8dc58791266179087907a2ff4cd883615216749b97d2f12173171c725a6f84a0000000002aeab5f01fff2aa5f01e0a9d0f2f08a01028fdb3d5b5a2c363d36ea17a4add99a23a3ec7935b4c3e1e0364fcc4295c7a2ef5f01f912b15f5d17c1539d4722f79d8856d8654c5af87f54cfb3a4ff7f6b512b2a08023c000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000f1755090c809421d69873c161e7969b8bf33cee3b451dd4859bfc244a705f0b4900498f804b6023e13fa023a0fb759e8b7c9a39506a21442bc47077beeedc6b78d34c4ebdae91bd96097ccc9a882bc5056568b0d2f1f06559368fea4acba8e745444e883e53156d5083c1fd260edf05292934c8b40c098b81fe4e261720bdd272b209e317247a1d2c55dc4718891af0d16273c5a610f36f382a3bf50f54808aaa6a508e51d4601dd0d8fbf8b3b1685066ce121666a1409e8ac7a4d673c1cc36d10b825f764af647441f53230518e4d2efbcf8791c6060912c76e90db4982a66d51bbd96290bbb34db8080b216c2940cec407260bf5e2c3a5ee280835f15298f0801e9d98c4d414792282fbc2c28c3e20bc0fcb1829b5c3ad8f8d20847be8fdb2a949fd96f0205fbd6d271c880c5d8c83e9813606cd803a44d377fdeae45bfa67112132af601e9b3b0613ba7dff2ec3d4b935c447b47bfe39f7b950981b2f4c66c0d853e2218f1f69229a9b608c3d98be09b6d4d640a9f6ff0e920dbacf7e58b59554c0b398b1ae4b1d497104b4e4e745d850eed7eddb8aa93437427bf442ae5beb22cbf10a8fa738ea38cfa5d86dfd30675d4be11a38016e36936fd5601e52643e8b8bc433702ea7ae6149309c95b898cc854850e73fe0b95c5b8879b7325ecd4",
    "difficulty": 61043624293,
    "expected_reward": 4771949057248,
    "height": 1561970,
    "prev_hash": "f8dc58791266179087907a2ff4cd883615216749b97d2f12173171c725a6f84a",
    "reserved_offset": 129,
    "status": "OK",
    "untrusted": false
  }
}
```

#### **submit\_block**

Submit a mined block to the network.

Alias: _submitblock_.

Inputs:

* Block blob data - array of strings; list of block blobs which have been mined. See  [get\_block\_template]()  to get a blob on which to mine.

Outputs:

* _status_  - string; Block submit status.

In this example, a block blob which has not been mined is submitted:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"submit_block","params":["0707e6bdfedc053771512f1bc27c62731ae9e8f2443db64ce742f4e57f5cf8d393de28551e441a0000000002fb830a01ffbf830a018cfe88bee283060274c0aae2ef5730e680308d9c00b6da59187ad0352efe3c71d36eeeb28782f29f2501bd56b952c3ddc3e350c2631d3a5086cac172c56893831228b17de296ff4669de020200000000"]' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "error": {
    "code": -7,
    "message": "Block not accepted"
  }
}
```

#### **get\_last\_block\_header**

Block header information for the most recent block is easily retrieved with this method. No inputs are needed.

Alias: _getlastblockheader_.

Inputs: _None_.

Outputs:

* _block\_header_  - A structure containing block header information.
  * _block\_size_  - unsigned int; The block size in bytes.
  * _depth_  - unsigned int; The number of blocks succeeding this block on the blockchain. A larger number means an older block.
  * _difficulty_  - unsigned int; The strength of the Oxen network
  * _hash_  - string; The hash of this block.
  * _height_  - unsigned int; The number of blocks preceding this block on the blockchain.
  * _major\_version_  - unsigned int; The major version of the Oxen protocol at this block height.
  * _minor\_version_  - unsigned int; The minor version of the Oxen protocol at this block height.
  * _nonce_  - unsigned int; a cryptographic random one-time number used in mining a $OXEN block.
  * _num\_txes_  - unsigned int; Number of transactions in the block, not counting the coinbase tx.
  * _orphan\_status_  - boolean; Usually  `false`. If  `true`, this block is not part of the longest chain.
  * _prev\_hash_  - string; The hash of the block immediately preceding this block in the chain.
  * _reward_  - unsigned int; The amount of new    generated in this block and rewarded to the miner. Note: 1 $OXEN = 1e12  atomic units.
  * _timestamp_  - unsigned int; The unix time at which the block was recorded into the blockchain.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

In this example, the most recent block \(1562023 at the time\) is returned:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_last_block_header"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 62774,
      "depth": 0,
      "difficulty": 60097900840,
      "hash": "3a289b8fa88b10e2163826c230b45d79f2be37d14fa3153ee58ff8a427782d14",
      "height": 1562023,
      "major_version": 7,
      "minor_version": 7,
      "nonce": 3789681204,
      "num_txes": 5,
      "orphan_status": false,
      "prev_hash": "743e5d0a26849efe27b96086f2c4ecc39a0bc744bf21473dad6710221aff6ac3",
      "reward": 4724029079703,
      "timestamp": 1525029411
    },
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_block\_header\_by\_hash**

Block header information can be retrieved using either a block's hash or height. This method includes a block's hash as an input parameter to retrieve basic information about the block.

Alias: _getblockheaderbyhash_.

Inputs:

* _hash_  - string; The block's sha256 hash.

Outputs:

* _block\_header_  - A structure containing block header information. See  [get\_last\_block\_header]().
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

In this example, block 912345 is looked up by its hash:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_header_by_hash","params":{"hash":"e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6"}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 210,
      "depth": 649717,
      "difficulty": 815625611,
      "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
      "height": 912345,
      "major_version": 1,
      "minor_version": 2,
      "nonce": 1646,
      "num_txes": 0,
      "orphan_status": false,
      "prev_hash": "b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff78",
      "reward": 7388968946286,
      "timestamp": 1452793716
    },
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_block\_header\_by\_height**

Similar to [get\_block\_header\_by\_hash]() above, this method includes a block's height as an input parameter to retrieve basic information about the block.

Alias: _getblockheaderbyheight_.

Inputs:

* _height_  - unsigned int; The block's height.

Outputs:

* _block\_header_  - A structure containing block header information. See  [get\_last\_block\_header]().
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

In this example, block 912345 is looked up by its height \(notice that the returned information is the same as in the previous example\):

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_header_by_height","params":{"height":912345}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "block_header": {
      "block_size": 210,
      "depth": 649721,
      "difficulty": 815625611,
      "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
      "height": 912345,
      "major_version": 1,
      "minor_version": 2,
      "nonce": 1646,
      "num_txes": 0,
      "orphan_status": false,
      "prev_hash": "b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff78",
      "reward": 7388968946286,
      "timestamp": 1452793716
    },
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_block\_headers\_range**

Similar to [get\_block\_header\_by\_height]() above, but for a range of blocks. This method includes a starting block height and an ending block height as parameters to retrieve basic information about the range of blocks.

Alias: _getblockheadersrange_.

Inputs:

* _start\_height_  - unsigned int; The starting block's height.
* _end\_height_  - unsigned int; The ending block's height.

Outputs:

* _headers_  - array of  `block_header`  \(a structure containing block header information. See  [get\_last\_block\_header]()\).
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

In this example, blocks range from height 1545999 to 1546000 is looked up \(notice that the returned information are ascending order and that it is at the April 2018 network upgrade time\):

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block_headers_range","params":{"start_height":1545999,"end_height":1546000}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "headers": [{
      "block_size": 301413,
      "depth": 16085,
      "difficulty": 134636057921,
      "hash": "86d1d20a40cefcf3dd410ff6967e0491613b77bf73ea8f1bf2e335cf9cf7d57a",
      "height": 1545999,
      "major_version": 6,
      "minor_version": 6,
      "nonce": 3246403956,
      "num_txes": 20,
      "orphan_status": false,
      "prev_hash": "0ef6e948f77b8f8806621003f5de24b1bcbea150bc0e376835aea099674a5db5",
      "reward": 5025593029981,
      "timestamp": 1523002893
    },{
      "block_size": 13322,
      "depth": 16084,
      "difficulty": 134716086238,
      "hash": "b408bf4cfcd7de13e7e370c84b8314c85b24f0ba4093ca1d6eeb30b35e34e91a",
      "height": 1546000,
      "major_version": 7,
      "minor_version": 7,
      "nonce": 3737164176,
      "num_txes": 1,
      "orphan_status": false,
      "prev_hash": "86d1d20a40cefcf3dd410ff6967e0491613b77bf73ea8f1bf2e335cf9cf7d57a",
      "reward": 4851952181070,
      "timestamp": 1523002931
    }],
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_block**

Full block information can be retrieved by either block height or hash, like with the above block header calls. For full block information, both lookups use the same method, but with different input parameters.

Alias: _getblock_.

Inputs \(pick one of the following\):

* _height_  - unsigned int; The block's height.
* _hash_  - string; The block's hash.

Outputs:

* _blob_  - string; Hexadecimal blob of block information.
* _block\_header_  - A structure containing block header information. See  [get\_last\_block\_header]().
* _json_  - json string; JSON formatted block details:
  * _major\_version_  - Same as in block header.
  * _minor\_version_  - Same as in block header.
  * _timestamp_  - Same as in block header.
  * _prev\_id_  - Same as  `prev_hash`  in block header.
  * _nonce_  - Same as in block header.
  * _miner\_tx_  - Miner transaction information
    * _version_  - Transaction version number.
    * _unlock\_time_  - The block height when the coinbase transaction becomes spendable.
    * _vin_  - List of transaction inputs:
      * _gen_  - Miner txs are coinbase txs, or "gen".
        * _height_  - This block height, a.k.a. when the coinbase is generated.
    * _vout_  - List of transaction outputs. Each output contains:
      * _amount_  - The amount of the output, in  atomic units.
      * _target_  -
        * _key_  -
    * _extra_  - Usually called the "transaction ID" but can be used to include any random 32 byte/64 character hex string.
    * _signatures_  - Contain signatures of tx signers. Coinbased txs do not have signatures.
  * _tx\_hashes_  - List of hashes of non-coinbase transactions in the block. If there are no other transactions, this will be an empty list.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

**Look up by height:**

In the following example, block 912345 is looked up by its height. Note that block 912345 does not have any non-coinbase transactions. \(See the next example for a block with extra transactions\):

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block","params":{"height":912345}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "blob": "0102f4bedfb405b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff786e0600000195d83701ffd9d73704ee84ddb42102378b043c1724c92c69d923d266fe86477d3a5ddd21145062e148c64c5767700880c0fc82aa020273733cbd6e6218bda671596462a4b062f95cfe5e1dbb5b990dacb30e827d02f280f092cbdd080247a5dab669770da69a860acde21616a119818e1a489bb3c4b1b6b3c50547bc0c80e08d84ddcb01021f7e4762b8b755e3e3c72b8610cc87b9bc25d1f0a87c0c816ebb952e4f8aff3d2b01fd0a778957f4f3103a838afda488c3cdadf2697b3d34ad71234282b2fad9100e02080000000bdfc2c16c00",
    "block_header": {
      "block_size": 210,
      "depth": 649772,
      "difficulty": 815625611,
      "hash": "e22cf75f39ae720e8b71b3d120a5ac03f0db50bba6379e2850975b4859190bc6",
      "height": 912345,
      "major_version": 1,
      "minor_version": 2,
      "nonce": 1646,
      "num_txes": 0,
      "orphan_status": false,
      "prev_hash": "b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff78",
      "reward": 7388968946286,
      "timestamp": 1452793716
    },
    "json": "{\n  \"major_version\": 1, \n  \"minor_version\": 2, \n  \"timestamp\": 1452793716, \n  \"prev_id\": \"b61c58b2e0be53fad5ef9d9731a55e8a81d972b8d90ed07c04fd37ca6403ff78\", \n  \"nonce\": 1646, \n  \"miner_tx\": {\n    \"version\": 1, \n    \"unlock_time\": 912405, \n    \"vin\": [ {\n        \"gen\": {\n          \"height\": 912345\n        }\n      }\n    ], \n    \"vout\": [ {\n        \"amount\": 8968946286, \n        \"target\": {\n          \"key\": \"378b043c1724c92c69d923d266fe86477d3a5ddd21145062e148c64c57677008\"\n        }\n      }, {\n        \"amount\": 80000000000, \n        \"target\": {\n          \"key\": \"73733cbd6e6218bda671596462a4b062f95cfe5e1dbb5b990dacb30e827d02f2\"\n        }\n      }, {\n        \"amount\": 300000000000, \n        \"target\": {\n          \"key\": \"47a5dab669770da69a860acde21616a119818e1a489bb3c4b1b6b3c50547bc0c\"\n        }\n      }, {\n        \"amount\": 7000000000000, \n        \"target\": {\n          \"key\": \"1f7e4762b8b755e3e3c72b8610cc87b9bc25d1f0a87c0c816ebb952e4f8aff3d\"\n        }\n      }\n    ], \n    \"extra\": [ 1, 253, 10, 119, 137, 87, 244, 243, 16, 58, 131, 138, 253, 164, 136, 195, 205, 173, 242, 105, 123, 61, 52, 173, 113, 35, 66, 130, 178, 250, 217, 16, 14, 2, 8, 0, 0, 0, 11, 223, 194, 193, 108\n    ], \n    \"signatures\": [ ]\n  }, \n  \"tx_hashes\": [ ]\n}",
    "miner_tx_hash": "c7da3965f25c19b8eb7dd8db48dcd4e7c885e2491db77e289f0609bf8e08ec30",
    "status": "OK",
    "untrusted": false
  }
}
```

**Look up by hash:**

In the following example, block 993056 is looked up by its hash. Note that block 993056 has 3 non-coinbase transactions:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_block","params":{"hash":"510ee3c4e14330a7b96e883c323a60ebd1b5556ac1262d0bc03c24a3b785516f"}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "blob": "0102a3978cb7050ea4af6547c05c965afc8df6d31509ff3105dc7ae6b10172521d77e09711fd6df407000001dcce3c01ffa0ce3c049da8bece070259e9d685b3484886bc7b47c133e6099ecdf212d5eaa16ce19cd58e8c3c1e590a80d88ee16f024c5e2f542d25513c46b9e3b7d40140a22d0ae5314bfcae492ad9f56fff8185f080d0b8e1981a0213dd8ffdac9e6a2f71e327dad65328198dc879a492d145eae72677c0703a351580c0f9decfae010262bda00341681dccbc066757862da593734395745bdfe1fdc89b5948c86a5d4c2b01b691851cf057b9c302a3dbca879e1cba4cc45061ca55aaa6e03cdc67ab9e455002080000000c617fdf160379c6b9f00db027bde151705aafe85c495883aae2597d5cb8e1adb2e0f3ae1d07d715db73331abc3ec588ef07c7bb195786a4724b08dff431b51ffa32a4ce899bb197066426c0ed89f0b431fe171f7fd62bc95dd29943daa7cf3585cf1fdfc99d",
    "block_header": {
      "block_size": 3981,
      "depth": 569068,
      "difficulty": 964985344,
      "hash": "510ee3c4e14330a7b96e883c323a60ebd1b5556ac1262d0bc03c24a3b785516f",
      "height": 993056,
      "major_version": 1,
      "minor_version": 2,
      "nonce": 2036,
      "num_txes": 3,
      "orphan_status": false,
      "prev_hash": "0ea4af6547c05c965afc8df6d31509ff3105dc7ae6b10172521d77e09711fd6d",
      "reward": 6932043647005,
      "timestamp": 1457720227
    },
    "json": "{\n  \"major_version\": 1, \n  \"minor_version\": 2, \n  \"timestamp\": 1457720227, \n  \"prev_id\": \"0ea4af6547c05c965afc8df6d31509ff3105dc7ae6b10172521d77e09711fd6d\", \n  \"nonce\": 2036, \n  \"miner_tx\": {\n    \"version\": 1, \n    \"unlock_time\": 993116, \n    \"vin\": [ {\n        \"gen\": {\n          \"height\": 993056\n        }\n      }\n    ], \n    \"vout\": [ {\n        \"amount\": 2043647005, \n        \"target\": {\n          \"key\": \"59e9d685b3484886bc7b47c133e6099ecdf212d5eaa16ce19cd58e8c3c1e590a\"\n        }\n      }, {\n        \"amount\": 30000000000, \n        \"target\": {\n          \"key\": \"4c5e2f542d25513c46b9e3b7d40140a22d0ae5314bfcae492ad9f56fff8185f0\"\n        }\n      }, {\n        \"amount\": 900000000000, \n        \"target\": {\n          \"key\": \"13dd8ffdac9e6a2f71e327dad65328198dc879a492d145eae72677c0703a3515\"\n        }\n      }, {\n        \"amount\": 6000000000000, \n        \"target\": {\n          \"key\": \"62bda00341681dccbc066757862da593734395745bdfe1fdc89b5948c86a5d4c\"\n        }\n      }\n    ], \n    \"extra\": [ 1, 182, 145, 133, 28, 240, 87, 185, 195, 2, 163, 219, 202, 135, 158, 28, 186, 76, 196, 80, 97, 202, 85, 170, 166, 224, 60, 220, 103, 171, 158, 69, 80, 2, 8, 0, 0, 0, 12, 97, 127, 223, 22\n    ], \n    \"signatures\": [ ]\n  }, \n  \"tx_hashes\": [ \"79c6b9f00db027bde151705aafe85c495883aae2597d5cb8e1adb2e0f3ae1d07\", \"d715db73331abc3ec588ef07c7bb195786a4724b08dff431b51ffa32a4ce899b\", \"b197066426c0ed89f0b431fe171f7fd62bc95dd29943daa7cf3585cf1fdfc99d\"\n  ]\n}",
    "miner_tx_hash": "372395aeac5e5ad2c40b4c546b0bad00c4242fb2bd88e2e25f4e43231876f81e",
    "status": "OK",
    "tx_hashes": ["79c6b9f00db027bde151705aafe85c495883aae2597d5cb8e1adb2e0f3ae1d07","d715db73331abc3ec588ef07c7bb195786a4724b08dff431b51ffa32a4ce899b","b197066426c0ed89f0b431fe171f7fd62bc95dd29943daa7cf3585cf1fdfc99d"],
    "untrusted": false
  }
}
```

#### **get\_connections**

Retrieve information about incoming and outgoing connections to your node.

Alias: _None_.

Inputs: _None_.

Outputs:

* _connections_  - List of all connections and their info:
  * _address_  - string; The peer's address, actually IPv4 & port
  * _avg\_download_  - unsigned int; Average bytes of data downloaded by node.
  * _avg\_upload_  - unsigned int; Average bytes of data uploaded by node.
  * _connection\_id_  - string; The connection ID
  * _current\_download_  - unsigned int; Current bytes downloaded by node.
  * _current\_upload_  - unsigned int; Current bytes uploaded by node.
  * _height_- unsigned int; The peer height
  * _host_  - string; The peer host
  * _incoming_  - boolean; Is the node getting information from your node?
  * _ip_  - string; The node's IP address.
  * _live\_time_  - unsigned int
  * _local\_ip_  - boolean
  * _localhost_  - boolean
  * _peer\_id_  - string; The node's ID on the network.
  * _port_  - string; The port that the node is using to connect to the network.
  * _recv\_count_  - unsigned int
  * _recv\_idle\_time_  - unsigned int
  * _send\_count_  - unsigned int
  * _send\_idle\_time_  - unsigned int
  * _state_  - string
  * _support\_flags_  - unsigned int

Following is an example of `get_connections` and it's return:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_connections"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "connections": [{
      "address": "173.90.69.136:62950",
      "avg_download": 0,
      "avg_upload": 2,
      "connection_id": "083c301a3030329a487adb12ad981d2c",
      "current_download": 0,
      "current_upload": 2,
      "height": 1562127,
      "host": "173.90.69.136",
      "incoming": true,
      "ip": "173.90.69.136",
      "live_time": 8,
      "local_ip": false,
      "localhost": false,
      "peer_id": "c959fbfbed9e44fb",
      "port": "62950",
      "recv_count": 259,
      "recv_idle_time": 8,
      "send_count": 24342,
      "send_idle_time": 8,
      "state": "state_normal",
      "support_flags": 0
    },{
    ...
    }],
    "status": "OK"
  }
}
```

#### **get\_info**

Retrieve general information about the state of your node and the network.

Alias:

* _/get\_info_
* _/getinfo_

See other RPC Methods [/get\_info \(not JSON\)]()

Inputs: _None_.

Outputs:

* _alt\_blocks\_count_  - unsigned int; Number of alternative blocks to main chain.
* _block\_size\_limit_  - unsigned int; Maximum allowed block size
* _block\_size\_median_  - unsigned int; Median block size of latest 100 blocks
* _bootstrap\_daemon\_address_  - string;  [bootstrap node](https://ww.getloki.org/resources/lokipedia/bootstrap-node.html)  to give immediate usability to wallets while syncing by proxying RPC to it. \(Note: the replies may be untrustworthy\).
* _cumulative\_difficulty_  - unsigned int; Cumulative difficulty of all blocks in the blockchain.
* _difficulty_  - unsigned int; Network difficulty \(analogous to the strength of the network\)
* _free\_space_  - unsigned int; Available disk space on the node.
* _grey\_peerlist\_size_  - unsigned int; Grey Peerlist Size
* _height_  - unsigned int; Current length of longest chain known to daemon.
* _height\_without\_bootstrap_  - unsigned int; Current length of the local chain of the daemon.
* _incoming\_connections\_count_  - unsigned int; Number of peers connected to and pulling from your node.
* _mainnet_  - boolean; States if the node is on the mainnet \(`true`\) or not \(`false`\).
* _offline_  - boolean; States if the node is offline \(`true`\) or online \(`false`\).
* _outgoing\_connections\_count_  - unsigned int; Number of peers that you are connected to and getting information from.
* _rpc\_connections\_count_  - unsigned int; Number of RPC client connected to the daemon \(Including this RPC request\).
* _stagenet_  - boolean; States if the node is on the stagenet \(`true`\) or not \(`false`\).
* _start\_time_  - unsigned int; Start time of the daemon, as UNIX time.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _target_  - unsigned int; Current target for next proof of work.
* _target\_height_  - unsigned int; The height of the next block in the chain.
* _testnet_  - boolean; States if the node is on the testnet \(`true`\) or not \(`false`\).
* _top\_block\_hash_  - string; Hash of the highest block in the chain.
* _tx\_count_  - unsigned int; Total number of non-coinbase transaction in the chain.
* _tx\_pool\_size_  - unsigned int; Number of transactions that have been broadcast but not included in a block.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).
* _was\_bootstrap\_ever\_used_  - boolean; States if a bootstrap node has ever been used since the daemon started.
* _white\_peerlist\_size_  - unsigned int; White Peerlist Size

Following is an example `get_info` call and its return:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_info"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "alt_blocks_count": 6,
    "block_size_limit": 600000,
    "block_size_median": 129017,
    "bootstrap_daemon_address": "",
    "cumulative_difficulty": 14121125493385685,
    "difficulty": 60580751777,
    "free_space": 138758750208,
    "grey_peerlist_size": 4998,
    "height": 1562168,
    "height_without_bootstrap": 1562168,
    "incoming_connections_count": 2,
    "mainnet": true,
    "offline": false,
    "outgoing_connections_count": 8,
    "rpc_connections_count": 2,
    "stagenet": false,
    "start_time": 1524751757,
    "status": "OK",
    "target": 120,
    "target_height": 1562063,
    "testnet": false,
    "top_block_hash": "7a7ba647080844073fdd8e3a069e00554c773d6e6863354dba1dec45a43f5592",
    "tx_count": 2759894,
    "tx_pool_size": 755,
    "untrusted": false,
    "was_bootstrap_ever_used": false,
    "white_peerlist_size": 1000
  }
}
```

#### **hard\_fork\_info**

Look up information regarding hard fork voting and readiness.

Alias: _None_.

Inputs: _None_.

Outputs:

* _earliest\_height_  - unsigned int; Block height at which hard fork would be enabled if voted in.
* _enabled_  - boolean; Tells if hard fork is enforced.
* _state_  - unsigned int; Current hard fork state: 0 \(There is likely a hard fork\), 1 \(An update is needed to fork properly\), or 2 \(Everything looks good\).
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _threshold_  - unsigned int; Minimum percent of votes to trigger hard fork. Default is 80.
* _version_  - unsigned int; The major block version for the fork.
* _votes_  - unsigned int; Number of votes towards hard fork.
* _voting_  - unsigned int; Hard fork voting status.
* _window_  - unsigned int; Number of blocks over which current votes are cast. Default is 10080 blocks.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"hard_fork_info"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "earliest_height": 1009827,
    "enabled": false,
    "state": 2,
    "status": "OK",
    "threshold": 0,
    "version": 1,
    "votes": 7277,
    "voting": 2,
    "window": 10080
  }
}
```

#### **set\_bans**

Ban another node by IP.

Alias: _None_.

Inputs:

* _bans_  - A list of nodes to ban:
  * _host_  - string; Host to ban \(IP in A.B.C.D form - will support I2P address in the future\).
  * _ip_  - unsigned int; IP address to ban, in Int format.
  * _ban_  - boolean; Set  `true`  to ban.
  * _seconds_  - unsigned int; Number of seconds to ban node.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Examples:

**banning by host**

In the following example, host is banned with its IP address string-formatted as A.B.C.D:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_bans","params":{"bans":[{"host":"192.168.1.51","ban":true,"seconds":30}]}}' -H  'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

**banning by ip**

In the following example, integer-formatted IP is banned:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"set_bans","params":{"bans":[{"ip":838969536,"ban":true,"seconds":30}]}}' -H  'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

#### **get\_bans**

Get list of banned IPs.

Alias: _None_.

Inputs: _None_.

Outputs:

* _bans_  - List of banned nodes:
  * _host_  - string; Banned host \(IP in A.B.C.D form\).
  * _ip_  - unsigned int; Banned IP address, in Int format.
  * _seconds_  - unsigned int; Local Unix time that IP is banned until.
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_bans"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "bans": [{
      "host": "102.168.1.51",
      "ip": 855746662,
      "seconds": 22
    },{
      "host": "192.168.1.50",
      "ip": 838969536,
      "seconds": 28
    }],
    "status": "OK"
  }
}
```

#### **flush\_txpool**

Flush tx ids from transaction pool

Alias: _None_.

Inputs:

* _txids_  - array of strings; Optional, list of transactions IDs to flush from pool \(all tx ids flushed if empty\).

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"flush_txpool","params":{"txids":["dc16fa8eaffe1484ca9014ea050e13131d3acf23b419f33bb4cc0b32b6c49308",""]}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

#### **get\_output\_histogram**

Get a histogram of output amounts. For all amounts \(possibly filtered by parameters\), gives the number of outputs on the chain for that amount. RingCT outputs counts as 0 amount.

Inputs:

* _amounts_  - list of unsigned int
* _min\_count_  - unsigned int
* _max\_count_  - unsigned int
* _unlocked_  - boolean
* _recent\_cutoff_  - unsigned int

Outputs:

* _histogram_  - list of histogram entries, in the following structure:
  * _amount_  - unsigned int; Output amount in  atomic units
  * _total\_instances_  - unsigned int;
  * _unlocked\_instances_  - unsigned int;
  * _recent\_instances_  - unsigned int;
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_output_histogram","params":{"amounts":[20000000000]}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "histogram": [{
      "amount": 20000000000,
      "recent_instances": 0,
      "total_instances": 381458,
      "unlocked_instances": 0
    }],
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_coinbase\_tx\_sum**

Get the coinbase amount and the fees amount for n last blocks starting at particular height

Alias: _None_.

Inputs:

* _height_  - unsigned int; Block height from which getting the amounts
* _count_  - unsigned int; number of blocks to include in the sum

Outputs:

* _emission\_amount_  - unsigned int; amount of coinbase reward in  atomic units
* _fee\_amount_  - unsigned int; amount of fees in  atomic units
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_coinbase_tx_sum","params":{"height":1563078,"count":2}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "emission_amount": 9387854817320,
    "fee_amount": 83981380000,
    "status": "OK"
  }
}
```

#### **get\_version**

Give the node current version

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).
* _version_  - unsigned int;

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_version"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK",
    "untrusted": false,
    "version": 65555
  }
}
```

#### **get\_fee\_estimate**

Gives an estimation on fees per kB.

Alias: _None_.

Inputs:

* _grace\_blocks_  - unsigned int; Optional

Outputs:

* _fee_  - unsigned int; Amount of fees estimated per kB in  atomic units
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_fee_estimate"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "fee": 187610000,
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_alternate\_chains**

Display alternative chains seen by the node.

Alias: _None_.

Inputs: _None_.

Outputs:

* _chains_  - array of chains, the following structure:
  * _block\_hash_  - string; the block hash of the first diverging block of this alternative chain.
  * _difficulty_  - unsigned int; the cumulative difficulty of all blocks in the alternative chain.
  * _height_  - unsigned int; the block height of the first diverging block of this alternative chain.
  * _length_  - unsigned int; the length in blocks of this alternative chain, after divergence.
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_alternate_chains"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "chains": [{
      "block_hash": "697cf03c89a9b118f7bdf11b1b3a6a028d7b3617d2d0ed91322c5709acf75625",
      "difficulty": 14114729638300280,
      "height": 1562062,
      "length": 2
    }],
    "status": "OK"
  }
}
```

#### **relay\_tx**

Relay a list of transaction IDs.

Alias: _None_.

Inputs:

* _txids_  - array of string; list of transaction IDs to relay

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"relay_tx","params":{"txids":[9fd75c429cbe52da9a52f2ffc5fbd107fe7fd2099c0d8de274dc8a67e0c98613]}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "status": "OK"
  }
}
```

#### **sync\_info**

Get synchronisation information

Alias: _None_.

Inputs: _None_.

Outputs:

* _height_  - unsigned int;
* _peers_  - array of peer structure, defined as follows:
  * _info_  - structure of connection info, as defined in  [get\_connections]()
* _spans_  - array of span structure, defined as follows \(optional, absent if node is fully synced\):
  * _connection\_id_  - string; Id of connection
  * _nblocks_  - unsigned int; number of blocks in that span
  * _rate_  - unsigned int; connection rate
  * _remote\_address_  - string; peer address the node is downloading \(or has downloaded\) than span from
  * _size_  - unsigned int; total number of bytes in that span's blocks \(including txes\)
  * _speed_  - unsigned int; connection speed
  * _start\_block\_height_  - unsigned int; block height of the first block in that span
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _target\_height_  - unsigned int; target height the node is syncing from \(optional, absent if node is fully synced\)

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"sync_info"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "height": 1563543,
    "peers": [{
      "info": {
        "address": "70.109.53.128:60064",
        "avg_download": 0,
        "avg_upload": 5,
        "connection_id": "204067223b9b3415c265dd25ad29ee48",
        "current_download": 0,
        "current_upload": 1,
        "height": 1559975,
        "host": "70.109.53.128",
        "incoming": true,
        "ip": "70.109.53.128",
        "live_time": 38,
        "local_ip": false,
        "localhost": false,
        "peer_id": "96b8545dbc7a8866",
        "port": "60064",
        "recv_count": 1580,
        "recv_idle_time": 28,
        "send_count": 203603,
        "send_idle_time": 8,
        "state": "state_normal",
        "support_flags": 1
      }
    },{
      "info": {
        ...
      }
    },{
      ...
    },{
      ...
    },{
      ...
    }],
    "status": "OK",
    "target_height": 1564067
  }
}
```

#### **get\_txpool\_backlog**

Get all transaction pool backlog

Alias: _None_.

Inputs: _None_.

Outputs:

* _backlog_: array of structures  _tx\_backlog\_entry_  \(in binary form\):
  * _blob\_size_  - unsigned int \(in binary form\)
  * _fee_  - unsigned int \(in binary form\)
  * _time\_in\_pool_  - unsigned int \(in binary form\)
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_txpool_backlog"}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "backlog": "...Binary...",
    "status": "OK",
    "untrusted": false
  }
}
```

#### **get\_output\_distribution**

Alias: _None_.

Inputs:

* _amounts_  - array of unsigned int; amounts to look for
* _cumulative_  - boolean; \(optional, default is  `false`\) States if the result should be cumulative \(`true`\) or not \(`false`\)
* _from\_height_  - unsigned int; \(optional, default is 0\) starting height to check from
* _to\_height_  - unsigned int; \(optional, default is 0\) ending height to check up to

Outputs:

* _distributions_  - array of structure distribution as follows:
  * _amount_  - unsigned int
  * _base_  - unsigned int
  * _distribution_  - array of unsigned int
  * _start\_height_  - unsigned int
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/json_rpc -d '{"jsonrpc":"2.0","id":"0","method":"get_output_distribution","params":{"amounts":[628780000],"from_height":1462078}}' -H 'Content-Type: application/json'

{
  "id": "0",
  "jsonrpc": "2.0",
  "result": {
    "distributions": [{
      "amount": 2628780000,
      "base": 0,
      "distribution": "",
      "start_height": 1462078
    }],
    "status": "OK"
  }
}
```

### Other Daemon RPC Calls

Not all daemon RPC calls use the JSON\_RPC interface. This section gives examples of these calls.

The data structure for these calls is different than the JSON RPC calls. Whereas the JSON RPC methods were called using the `/json_rpc` extension and specifying a method, these methods are called at their own extensions. For example:

```text
IP=127.0.0.1
PORT=18081
METHOD='gettransactions'
PARAMS='{"txs_hashes":["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"]}'
curl \
  -X POST http://$IP:$PORT/$METHOD \
  -d $PARAMS \
  -H 'Content-Type: application/json'
```

Note: It is recommended to use JSON RPC where such alternatives exist, rather than the following methods. For example, the recommended way to get a node's height is via the JSON RPC methods [get\_info]() or [get\_last\_block\_header](), rather than [getheight]() below.

For calls that end with **.bin**, the data is exchanged in the form of binary, serialized objects, as defined in the Core RPC Server.

#### **/get\_height**

Get the node's current height.

Alias: _/getheight_.

Inputs: _None_.

Outputs:

* _height_  - unsigned int; Current length of longest chain known to daemon.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

```text
$ curl -X POST http://127.0.0.1:18081/get_height -H 'Content-Type: application/json'

{
  "height": 1564055,
  "status": "OK",
  "untrusted": false
}
```

#### **/get\_blocks.bin**

Get all blocks info. Binary request.

Alias: _/getblocks.bin_.

Inputs:

* _block\_ids_  - binary array of hashes; first 10 blocks id goes sequential, next goes in pow\(2,n\) offset, like 2, 4, 8, 16, 32, 64 and so on, and the last one is always genesis block
* _start\_height_  - unsigned int
* _prune_  - boolean

Outputs:

* _blocks_  - array of block complete entries
* _current\_height_  - unsigned int
* _output\_indices_  - structure as follows:
  * _indices_  - array of tx output indices, structure as follows:
    * _indices_  - array of unsigned int
* _start\_height_  - unsigned int
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/get\_blocks\_by\_height.bin**

Get blocks by height. Binary request.

Alias: _/getblocks\_by\_height.bin_.

Inputs:

* _heights_  - array of unsigned int; list of block heights

Outputs:

* _blocks_  - array of block complete entries
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/get\_hashes.bin**

Get hashes. Binary request.

Alias: _/gethashes.bin_.

Inputs:

* _block\_ids_  - binary array of hashes; first 10 blocks id goes sequential, next goes in pow\(2,n\) offset, like 2, 4, 8, 16, 32, 64 and so on, and the last one is always genesis block
* _start\_height_  - unsigned int

Outputs:

* _current\_height_  - unsigned int
* _m\_block\_ids_  - binary array of hashes; see  _block\_ids_  above.
* _start\_height_  - unsigned int
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/get\_o\_indexes.bin**

Get global outputs of transactions. Binary request.

Alias: _None_.

Inputs:

* _txid_  - binary txid

Outputs:

* _o\_indexes_  - array of unsigned int; List of output indexes
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/get\_outs.bin**

Get outputs. Binary request.

Alias: _None_.

Inputs:

* _outputs_  - array of structure  _get\_outputs\_out_  as follows:
  * _amount_  - unsigned int;
  * _index_  - unsigned int;

Outputs:

* _outs_  - array of structure  _outkey_  as follows:
  * _amount_  - unsigned int;
  * _height_  - unsigned int; block height of the output
  * _key_  - the public key of the output
  * _mask_
  * _txid_  - transaction id
  * _unlocked_  - boolean; States if output is locked \(`false`\) or not \(`true`\)
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/get\_transactions**

Look up one or more transactions by hash.

Alias: _/gettransactions_.

Inputs:

* _txs\_hashes_  - string list; List of transaction hashes to look up.
* _decode\_as\_json_  - boolean; Optional \(`false`  by default\). If set  `true`, the returned transaction information will be decoded rather than binary.
* _prune_  - boolean; Optional \(`false`  by default\).

Outputs:

* _missed\_tx_  - array of strings. \(Optional - returned if not empty\) Transaction hashes that could not be found.
* _status_  - General RPC error code. "OK" means everything looks good.
* _txs_  - array of structure  _entry_  as follows:
  * _as\_hex_  - string; Full transaction information as a hex string.
  * _as\_json_  - json string; List of transaction info:
    * _version_  - Transaction version
    * _unlock\_time_  - If not 0, this tells when a transaction output is spendable.
    * _vin_  - List of inputs into transaction:
      * _key_  - The public key of the previous output spent in this transaction.
        * _amount_  - The amount of the input, in  atomic units.
        * _key\_offsets_  - A list of integer offets to the input.
        * _k\_image_  - The key image for the given input
    * _vout_  - List of outputs from transaction:
      * _amount_  - Amount of transaction output, in  atomic units.
      * _target_  - Output destination information:
        * _key_  - The stealth public key of the receiver. Whoever owns the private key associated with this key controls this transaction output.
    * _extra_  - Usually called the "payment ID" but can be used to include any random 32 bytes.
    * _signatures_  - List of signatures used in ring signature to hide the true origin of the transaction.
  * _block\_height_  - unsigned int; block height including the transaction
  * _block\_timestamp_  - unsigned int; Unix time at chich the block has been added to the blockchain
  * _double\_spend\_seen_  - boolean; States if the transaction is a double-spend \(`true`\) or not \(`false`\)
  * _in\_pool_  - boolean; States if the transaction is in pool \(`true`\) or included in a block \(`false`\)
  * _output\_indices_  - array of unsigned int; transaction indexes
  * _tx\_hash_  - string; transaction hash
* _txs\_as\_hex_  - string; Full transaction information as a hex string \(old compatibility parameter\)
* _txs\_as\_json_  - json string; \(Optional - returned if set in inputs. Old compatibility parameter\) List of transaction as in  _as\_json_  above:

Example 1: Return transaction information in binary format.

```text
$ curl -X POST http://127.0.0.1:18081/get_transactions -d '{"txs_hashes":["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"]}' -H 'Content-Type: application/json'

{
  "status": "OK",
  "txs": [{
    "as_hex": "...",
    "as_json": "",
    "block_height": 993442,
    "block_timestamp": 1457749396,
    "double_spend_seen": false,
    "in_pool": false,
    "output_indices": [198769,418598,176616,50345,509],
    "tx_hash": "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
  }],
  "txs_as_hex": ["..."],
  "untrusted": false
}
```

Example 2: Decode returned transaction information in JSON format. Note: the "vin", "vout" and "signatures" list have been truncated in the displayed return for space considerations.

```text
$ curl -X POST http://127.0.0.1:18081/get_transactions -d '{"txs_hashes":["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"],"decode_as_json":true}' -H 'Content-Type: application/json'

{
  "status": "OK",
  "txs": [{
    "as_hex": "...",
    "as_json": "{\n  \"version\": 1, \n  \"unlock_time\": 0, \n  \"vin\": [ {\n      \"key\": {\n        \"amount\": 9999999999, \n        \"key_offsets\": [ 691\n        ], \n        \"k_image\": \"6ebee1b651a8da723462b4891d471b990ddc226049a0866d3029b8e2f75b7012\"\n      }\n    }, {\n      \"key\": {\n        \"amount\": 9000000000000, \n        \"key_offsets\": [ 175760\n        ], \n        \"k_image\": \"200bd02b70ee707441a8863c5279b4e4d9f376dc97a140b1e5bc7d72bc508069\"\n      }\n    }, ... \n  ], \n  \"vout\": [ {\n      \"amount\": 60000000000, \n      \"target\": {\n        \"key\": \"8c792dea94dab48160e067fb681edd6247ba375281fbcfedc03cb970f3b98e2d\"\n      }\n    }, {\n      \"amount\": 700000000000, \n      \"target\": {\n        \"key\": \"1ab33e69737e157d23e33274c42793be06a8711670e73fa10ecebc604f87cc71\"\n      }\n    }, ... \n  ], \n  \"extra\": [ 1, 3, 140, 109, 156, 205, 47, 148, 153, 9, 17, 93, 83, 33, 162, 110, 152, 1, 139, 70, 121, 19, 138, 10, 44, 6, 55, 140, 242, 124, 143, 219, 172\n  ], \n  \"signatures\": [ \"fd82214a59c99d9251fa00126d353f9cf502a80d8993a6c223e3c802a40ab405555637f495903d3ba558312881e586d452e6e95826d8e128345f6c0a8f9f350e\", \"8c04ef50cf34afa3a9ec19c457143496f8cf7045ed869b581f9efa2f1d65e30f1cec5272b00e9c61a34bdd3c78cf82ae8ef4df3132f70861391069b9c255cd08\", ... ]\n}",
    "block_height": 993442,
    "block_timestamp": 1457749396,
    "double_spend_seen": false,
    "in_pool": false,
    "output_indices": [198769,418598,176616,50345,509],
    "tx_hash": "d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090408"
  }],
  "txs_as_hex": ["..."],
  "txs_as_json": ["{\n  \"version\": 1, \n  \"unlock_time\": 0, \n  \"vin\": [ {\n      \"key\": {\n        \"amount\": 9999999999, \n        \"key_offsets\": [ 691\n        ], \n        \"k_image\": \"6ebee1b651a8da723462b4891d471b990ddc226049a0866d3029b8e2f75b7012\"\n      }\n    }, {\n      \"key\": {\n        \"amount\": 9000000000000, \n        \"key_offsets\": [ 175760\n        ], \n        \"k_image\": \"200bd02b70ee707441a8863c5279b4e4d9f376dc97a140b1e5bc7d72bc508069\"\n      }\n    }, ... \n  ], \n  \"vout\": [ {\n      \"amount\": 60000000000, \n      \"target\": {\n        \"key\": \"8c792dea94dab48160e067fb681edd6247ba375281fbcfedc03cb970f3b98e2d\"\n      }\n    }, {\n      \"amount\": 700000000000, \n      \"target\": {\n        \"key\": \"1ab33e69737e157d23e33274c42793be06a8711670e73fa10ecebc604f87cc71\"\n      }\n    }, ... \n  ], \n  \"extra\": [ 1, 3, 140, 109, 156, 205, 47, 148, 153, 9, 17, 93, 83, 33, 162, 110, 152, 1, 139, 70, 121, 19, 138, 10, 44, 6, 55, 140, 242, 124, 143, 219, 172\n  ], \n  \"signatures\": [ \"fd82214a59c99d9251fa00126d353f9cf502a80d8993a6c223e3c802a40ab405555637f495903d3ba558312881e586d452e6e95826d8e128345f6c0a8f9f350e\", \"8c04ef50cf34afa3a9ec19c457143496f8cf7045ed869b581f9efa2f1d65e30f1cec5272b00e9c61a34bdd3c78cf82ae8ef4df3132f70861391069b9c255cd08\", ... ]\n}"],
  "untrusted": false
}
```

Example 3: Returned a missed \(unexisting\) transaction.

```text
curl -X POST http://127.0.0.1:18081/get_transactions -d '{"txs_hashes":["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090409"]}' -H 'Content-Type: application/json'

{
  "missed_tx": ["d6e48158472848e6687173a91ae6eebfa3e1d778e65252ee99d7515d63090409"],
  "status": "OK",
  "untrusted": false
}
```

#### **/get\_alt\_blocks\_hashes**

Get the known blocks hashes which are not on the main chain.

Alias: _None_.

Inputs: _None_

Outputs:

* _blks\_hashes_  - array of strings; list of alternative blocks hashes to main chain
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/get_alt_blocks_hashes -H 'Content-Type: application/json'

{
  "blks_hashes": ["9c2277c5470234be8b32382cdf8094a103aba4fcd5e875a6fc159dc2ec00e011","637c0e0f0558e284493f38a5fcca3615db59458d90d3a5eff0a18ff59b83f46f","6f3adc174a2e8082819ebb965c96a095e3e8b63929ad9be2d705ad9c086a6b1c","697cf03c89a9b118f7bdf11b1b3a6a028d7b3617d2d0ed91322c5709acf75625","d99b3cf3ac6f17157ac7526782a3c3b9537f89d07e069f9ce7821d74bd9cad0e","e97b62109a6303233dcd697fa8545c9fcbc0bf8ed2268fede57ddfc36d8c939c","70ff822066a53ad64b04885c89bbe5ce3e537cdc1f7fa0dc55317986f01d1788","b0d36b209bd0d4442b55ea2f66b5c633f522401f921f5a85ea6f113fd2988866"],
  "status": "OK",
  "untrusted": false
}
```

#### **/is\_key\_image\_spent**

Check if outputs have been spent using the key image associated with the output.

Alias: _None_.

Inputs:

* _key\_images_  - string list; List of key image hex strings to check.

Outputs:

* _spent\_status_  - unsigned int list; List of statuses for each image checked. Statuses are follows: 0 = unspent, 1 = spent in blockchain, 2 = spent in transaction pool
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/is_key_image_spent -d '{"key_images":["8d1bd8181bf7d857bdb281e0153d84cd55a3fcaa57c3e570f4a49f935850b5e3","7319134bfc50668251f5b899c66b005805ee255c136f0e1cecbb0f3a912e09d4"]}' -H 'Content-Type: application/json'

{
  "spent_status": [1,2],
  "status": "OK"
  "untrusted": false
}
```

#### **/send\_raw\_transaction**

Broadcast a raw transaction to the network.

Alias: _/sendrawtransaction_.

Inputs:

* _tx\_as\_hex_  - string; Full transaction information as hexadecimal string.
* _do\_not\_relay_  - boolean; Stop relaying transaction to other nodes \(default is  `false`\).

Outputs:

* _double\_spend_  - boolean; Transaction is a double spend \(`true`\) or not \(`false`\).
* _fee\_too\_low_  - boolean; Fee is too low \(`true`\) or OK \(`false`\).
* _invalid\_input_  - boolean; Input is invalid \(`true`\) or valid \(`false`\).
* _invalid\_output_  - boolean; Output is invalid \(`true`\) or valid \(`false`\).
* _low\_mixin_  - boolean; Mixin count is too low \(`true`\) or OK \(`false`\).
* _not\_rct_  - boolean; Transaction is a standard ring transaction \(`true`\) or a ring confidential transaction \(`false`\).
* _not\_relayed_  - boolean; Transaction was not relayed \(`true`\) or relayed \(`false`\).
* _overspend_  - boolean; Transaction uses more money than available \(`true`\) or not \(`false`\).
* _reason_  - string; Additional information. Currently empty or "Not relayed" if transaction was accepted but not relayed.
* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.
* _too\_big_  - boolean; Transaction size is too big \(`true`\) or OK \(`false`\).
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example \(No return information included here.\):

```text
$ curl -X POST http://127.0.0.1:18081/send_raw_transaction -d '{"tx_as_hex":"de6a3...", "do_not_relay":false}' -H 'Content-Type: application/json'
```

#### **/save\_bc**

Save the blockchain. The blockchain does not need saving and is always saved when modified, however it does a sync to flush the filesystem cache onto the disk for safety purposes against Operating System or Hardware crashes.

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/save_bc -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/get\_peer\_list**

Get the known peers list.

Alias: _None_.

Inputs: _None_.

Outputs:

* _gray\_list_  - array of offline  _peer_  structure as follows:
  * _host_  - unsigned int; IP address in integer format
  * _id_  - string; Peer id
  * _ip_  - unsigned int; IP address in integer format
  * _last\_seen_  - unsigned int; unix time at which the peer has been seen for the last time
  * _port_  - unsigned int; TCP port the peer is using to connect to Oxen network.
* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.
* _white\_list_  - array of online  _peer_  structure, as above.

Example \(truncated lists\):

```text
$ curl -X POST http://127.0.0.1:18081/get_peer_list -H 'Content-Type: application/json'

{
  "gray_list": [{
    "host": "640304833",
    "id": 5345237316225602120,
    "ip": 640304833,
    "last_seen": 1525540510,
    "port": 18080
  },{
    "host": "2183731038",
    "id": 14955030573998424430,
    "ip": 2183731038,
    "last_seen": 1525540499,
    "port": 28080
  }, ...
  ],
  "status": "OK",
  "white_list": [{
    "host": "1221637955",
    "id": 10354694710033118926,
    "ip": 1221637955,
    "last_seen": 1525540511,
    "port": 18080
  },{
    "host": "1780407354",
    "id": 17193661050352240890,
    "ip": 1780407354,
    "last_seen": 1525540510,
    "port": 18080
  }, ...
  ]
}
```

#### **/set\_log\_hash\_rate**

Set the log hash rate display mode.

Alias: _None_.

Inputs:

* _visible_  - boolean; States if hash rate logs should be visible \(`true`\) or hidden \(`false`\)

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.

Example while mining:

```text
$ curl -X POST http://127.0.0.1:18081/set_log_hash_rate -d '{"visible":true}' -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

Error while not mining:

```text
$ curl -X POST http://127.0.0.1:18081/set_log_hash_rate -d '{"visible":true}' -H 'Content-Type: application/json'

{
  "status": "NOT MINING"
}
```

#### **/set\_log\_level**

Set the daemon log level. By default, log level is set to `0`.

Alias: _None_.

Inputs:

* _level_  - integer; daemon log level to set from  `0`  \(less verbose\) to  `4`  \(most verbose\)

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/set_log_level -d '{"level":1}' -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/set\_log\_categories**

Set the daemon log categories. Categories are represented as a comma separated list of `<Category>:<level>` \(similarly to syslog standard `<Facility>:<Severity-level>`\), where:

* _Category_  is one of the following:
  * _\*_  - All facilities
  * _default_
  * _net_
  * _net.http_
  * _net.p2p_
  * _logging_
  * _net.throttle_
  * _blockchain.db_
  * _blockchain.db.lmdb_
  * _bcutil_
  * _checkpoints_
  * _net.dns_
  * _net.dl_
  * _i18n_
  * _perf_
  * _stacktrace_
  * _updates_
  * _account_
  * _cn_
  * _difficulty_
  * _hardfork_
  * _miner_
  * _blockchain_
  * _txpool_
  * _cn.block\_queue_
  * _net.cn_
  * _daemon_
  * _debugtools.deserialize_
  * _debugtools.objectsizes_
  * _device.ledger_
  * _wallet.gen\_multisig_
  * _multisig_
  * _bulletproofs_
  * _ringct_
  * _daemon.rpc_
  * _wallet.simplewallet_
  * _WalletAPI_
  * _wallet.ringdb_
  * _wallet.wallet2_
  * _wallet.rpc_
  * _tests.core_
* _Level_  is one of the following:
  * _FATAL_  - higher level
  * _ERROR_
  * _WARNING_
  * _INFO_
  * _DEBUG_
  * _TRACE_  - lower level A level automatically includes higher level. By default, categories are set to  `*:WARNING,net:FATAL,net.p2p:FATAL,net.cn:FATAL,global:INFO,verify:FATAL,stacktrace:INFO,logging:INFO,msgwriter:INFO`. Setting the categories to "" prevent any logs to be outputted.

Alias: _None_.

Inputs:

* _categories_  - string; Optional, daemon log categories to enable

Outputs:

* _categories_  - string; daemon log enabled categories
* _status_  - string; General RPC error code. "OK" means everything looks good. Any other value means that something went wrong.

Example to set all facilities to Security Level `Info`:

```text
$ curl -X POST http://127.0.0.1:18081/set_log_categories -d '{"categories": "*:INFO"}' -H 'Content-Type: application/json'

{
  "categories": "*:INFO",
  "status": "OK"
}
```

Example without input to set the default categories:

```text
$ curl -X POST http://127.0.0.1:18081/set_log_categories -H 'Content-Type: application/json'

{
  "categories": "*:WARNING,net:FATAL,net.p2p:FATAL,net.cn:FATAL,global:INFO,verify:FATAL,stacktrace:INFO,logging:INFO,msgwriter:INFO",
  "status": "OK"
}
```

#### **/get\_transaction\_pool**

Show information about valid transactions seen by the node but not yet mined into a block, as well as spent key image information for the txpool in the node's memory.

Alias: _None_.

Inputs: _None_.

Outputs:

* _spent\_key\_images_  - List of spent output key images:
  * _id\_hash_  - string; Key image.
  * _txs\_hashes_  - string list; tx hashes of the txes \(usually one\) spending that key image.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _transactions_  - List of transactions in the mempool are not in a block on the main chain at the moment:
  * _blob\_size_  - unsigned int; The size of the full transaction blob.
  * _double\_spend\_seen_  - boolean; States if this transaction has been seen as double spend.
  * _do\_not\_relay_; boolean; States if this transaction should not be relayed
  * _fee_  - unsigned int; The amount of the mining fee included in the transaction, in  atomic units.
  * _id\_hash_  - string; The transaction ID hash.
  * _kept\_by\_block_  - boolean; States if the tx was included in a block at least once \(`true`\) or not \(`false`\).
  * _last\_failed\_height_  - unsigned int; If the transaction validation has previously failed, this tells at what height that occurred.
  * _last\_failed\_id\_hash_  - string; Like the previous, this tells the previous transaction ID hash.
  * _last\_relayed\_time_  - unsigned int; Last unix time at which the transaction has been relayed.
  * _max\_used\_block\_height_  - unsigned int; Tells the height of the most recent block with an output used in this transaction.
  * _max\_used\_block\_hash_  - string; Tells the hash of the most recent block with an output used in this transaction.
  * _receive\_time_  - unsigned int; The Unix time that the transaction was first seen on the network by the node.
  * _relayed_  - boolean; States if this transaction has been relayed
  * _tx\_blob_  - unsigned int; Hexadecimal blob represnting the transaction.
  * _tx\_json_  - json string; JSON structure of all information in the transaction:
    * _version_  - Transaction version
    * _unlock\_time_  - If not 0, this tells when a transaction output is spendable.
    * _vin_  - List of inputs into transaction:
      * _key_  - The public key of the previous output spent in this transaction.
        * _amount_  - The amount of the input, in  atomic units.
        * _key\_offsets_  - A list of integer offets to the input.
        * _k\_image_  - The key image for the given input
    * _vout_  - List of outputs from transaction:
      * _amount_  - Amount of transaction output, in  atomic units.
      * _target_  - Output destination information:
        * _key_  - The stealth public key of the receiver. Whoever owns the private key associated with this key controls this transaction output.
    * _extra_  - Usually called the "transaction ID" but can be used to include any random 32 bytes.
    * _rct\_signatures_  - Ring signatures:
      * _type_
      * _txnFee_
      * _ecdhInfo_  - array of Diffie Helman Elipctic curves structures as follows:
        * _mask_  - String
        * _amount_  - String
      * _outPk_
    * _rctsig\_prunable_
      * _rangeSigs_  - array of structures as follows:
        * _asig_
        * _Ci_
      * _MGs_  - array of structures as follows:
        * _ss_  - array of arrays of two strings.
        * _cc_  - String

Example \(Note: Some lists in the returned information have been truncated for display reasons\):

```text
$ curl -X POST http://127.0.0.1:18081/get_transaction_pool -H 'Content-Type: application/json'

{
  "spent_key_images": [{
    "id_hash": "a2af919609db4ff5ab8d4ba18502e647d521760e1cbc30288f06fa87bf9a0c1c",
    "txs_hashes": ["1ee6a4873b638711795fc3b0b73fc7146505a09a7f4749534fd408d571a273cf"]
  },{
    "id_hash": "02d5f6559e9bca5ae5a335130aeeb05df2db518ab9837fa64ebbab276c100792",
    "txs_hashes": ["531aacc0ceb8514cdde5f104285202ccc3e969c77584e3c6fa614c987c583965"]
  },
  ...],
  "status": "OK",
  "transactions": [{
    "blob_size": 13193,
    "do_not_relay": false,
    "double_spend_seen": false,
    "fee": 9694360000,
    "id_hash": "f8fb875cfc9e2e59bcf96a42474c79e01d50b69e6548d445d45984f7db66e50f",
    "kept_by_block": false,
    "last_failed_height": 0,
    "last_failed_id_hash": "0000000000000000000000000000000000000000000000000000000000000000",
    "last_relayed_time": 1525615049,
    "max_used_block_height": 1564924,
    "max_used_block_id_hash": "4bae7856979f46c7de31f3fb58cac36d4dfd2765bf33f876edf33d0e05ebb4a7",
    "receive_time": 1525615049,
    "relayed": true,
    "tx_blob": " ... ",
    "tx_json": "{\n  \"version\": 2, \n  \"unlock_time\": 0, \n  \"vin\": [ {\n      \"key\": {\n        \"amount\": 0, \n        \"key_offsets\": [ 2630347, 594429, 1047509, 758973, 464501, 61971, 22268\n        ], \n        \"k_image\": \"0731363c58dd4492f031fa20c82fe6ddcb9cc070d73938afe8a5f7f77897f8b4\"\n      }\n    }\n  ], \n  \"vout\": [ {\n      \"amount\": 0, \n      \"target\": {\n        \"key\": \"f3b3dd09483616e343b9866eed50a0ce01d5c0d0f2612ce2c4d0e9cce5c218cd\"\n      }\n    }, {\n      \"amount\": 0, \n      \"target\": {\n        \"key\": \"9796f2d477a696b6282bf3cb1a41cefba0c4604eedcc2e7a44904d7033643e0e\"\n      }\n    }\n  ], \n  \"extra\": [ 1, 25, 228, 80, 5, 214, 117, 150, 9, 125, 98, 17, 113, 208, 89, 223, 242, 227, 188, 197, 141, 190, 135, 140, 152, 117, 240, 150, 21, 93, 62, 108, 124\n  ], \n  \"rct_signatures\": {\n    \"type\": 1, \n    \"txnFee\": 9694360000, \n    \"ecdhInfo\": [ {\n        \"mask\": \"645f06a2816aecf83d5041c3320eb31092b994fb2733bb74c8c47e288d452c04\", \n        \"amount\": \"3908f14d39dcb3831331cb255eeadc5b0aea0143645b9cd3034abf613995740d\"\n      }, {\n        \"mask\": \"0785b5df0a994b14d59da810503a022721d8f629720f526e15bd848ad3c2c509\", \n        \"amount\": \"fbd81cf2368dcd742905ded5287457030467aaf5bc9939e13f1d6bf8d4c8ca09\"\n      }], \n    \"outPk\": [ \"c19f5fa052859126e0eed0e3c860aadab049677b2b3dd14cc74d02f92f1d013f\", \"1581ef6368de1608ea366566b88272db220479cf215f6d88d7b60ec221d11e0a\"]\n  }, \n  \"rctsig_prunable\": {\n    \"rangeSigs\": [ {\n        \"asig\": \" ... \", \n        \"Ci\": \" .. \"\n      }, {\n        \"asig\": \" ... \", \n        \"Ci\": \" ... \"\n      }], \n    \"MGs\": [ {\n        \"ss\": [ [ \"218a10a29e0f66e5a324af67b7734708a8a4cc8f16b28acd8cda538aaa495a02\", \"b368b4e956df5808c5c257f0dc3f7eff8c28463d0bb20759d19977fa02d6f205\"], [ \"f741d2c96bc23b362b4155a03fb6f1351ab5bf4445a43b3e52ba776f526af305\", \"a10ad1ee80dce3f311dd3dc141803daeecaa4d2a25a390cd9c35e4161b7c9e0c\"],
    ...], \n        \"cc\": \"e93801b707261ca76e146fdf2085abae71ad9203a00edc843c74f4ead8a39601\"\n      }]\n  }\n}"
  },
  ...]
}
```

#### **/get\_transaction\_pool\_hashes.bin**

Get hashes from transaction pool. Binary request.

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.
* _tx\_hashes_  - binary array of transaction hashes.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/get_transaction_pool_hashes.bin -H 'Content-Type: application/json'

{
  "status": "OK",
  "tx_hashes": " ... ",
  "untrusted": false
}
```

#### **/get\_transaction\_pool\_stats**

Get the transaction pool statistics.

Alias: _None_.

Inputs: _None_.

Outputs:

* _pool\_stats_  - Structure as follows:
  * _bytes\_max_  - unsigned int; Max transaction size in pool
  * _bytes\_med_  - unsigned int; Median transaction size in pool
  * _bytes\_min_  - unsigned int; Min transaction size in pool
  * _bytes\_total_  - unsigned int; total size of all transactions in pool
  * _histo_  - structure  _txpool\_histo_  as follows:
    * _txs_  - unsigned int; number of transactions
    * _bytes_  - unsigned int; size in bytes.
  * _histo\_98pc_  unsigned int; the time 98% of txes are "younger" than
  * _num\_10m_  unsigned int; number of transactions in pool for more than 10 minutes
  * _num\_double\_spends_  unsigned int; number of double spend transactions
  * _num\_failing_  unsigned int; number of failing transactions
  * _num\_not\_relayed_  unsigned int; number of non-relayed transactions
  * _oldest_  unsigned int; unix time of the oldest transaction in the pool
  * _txs\_total_  unsigned int; total number of transactions.
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/get_transaction_pool_stats -H 'Content-Type: application/json'

{
  "pool_stats": {
    "bytes_max": 47222,
    "bytes_med": 13290,
    "bytes_min": 13092,
    "bytes_total": 449511,
    "fee_total": 289715320000,
    "histo": "\t▒'▒5▒4▒\/▒▒▒$3",
    "histo_98pc": 0,
    "num_10m": 18,
    "num_double_spends": 1,
    "num_failing": 17,
    "num_not_relayed": 0,
    "oldest": 1525457001,
    "txs_total": 26
  },
  "status": "OK",
  "untrusted": false
}
```

#### **/stop\_daemon**

Send a command to the daemon to safely disconnect and shut down.

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/stop_daemon -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/get\_info \(not JSON\)**

This method is a convenient backward support and should not be used anymore. See [get\_info]() JSON RPC for details.

Alias:

* _/getinfo_
* _get\_info_

#### **/get\_limit**

Get daemon bandwidth limits.

Alias: _None_.

Inputs: _None_.

Outputs:

* _limit\_down_  - unsigned int; Download limit in kBytes per second
* _limit\_up_  - unsigned int; Upload limit in kBytes per second
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

Example:

```text
$ curl -X POST http://127.0.0.1:18081/get_limit -H 'Content-Type: application/json'

{
  "limit_down": 8192,
  "limit_up": 128,
  "status": "OK",
  "untrusted": false
}
```

#### **/set\_limit**

Set daemon bandwidth limits.

Alias: _None_.

Inputs:

* _limit\_down_  - signed int; Download limit in kBytes per second \(-1 reset to default, 0 don't change the current limit\)
* _limit\_up_  - signed int; Upload limit in kBytes per second \(-1 reset to default, 0 don't change the current limit\)

Outputs:

* _limit\_down_  - unsigned int; Download limit in kBytes per second
* _limit\_up_  - unsigned int; Upload limit in kBytes per second
* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/set_limit -d '{"limit_down": 1024}' -H 'Content-Type: application/json'

{
  "limit_down": 1024,
  "limit_up": 128,
  "status": "OK"
}
```

#### **/out\_peers**

Limit number of Outgoing peers.

Alias: _None_.

Inputs:

* _out\_peers_  - unsigned int; Max number of outgoing peers

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/out_peers -d '{"out_peers": 3232235535}' -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/in\_peers**

Limit number of Incoming peers.

Alias: _None_.

Inputs:

* _in\_peers_  - unsigned int; Max number of incoming peers

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/out_peers -d '{"in_peers": 3232235535}' -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/start\_save\_graph**

Obsolete. Conserved here for reference.

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/start_save_graph -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/stop\_save\_graph**

Obsolete. Conserved here for reference.

Alias: _None_.

Inputs: _None_.

Outputs:

* _status_  - string; General RPC error code. "OK" means everything looks good.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/stop_save_graph -H 'Content-Type: application/json'

{
  "status": "OK"
}
```

#### **/get\_outs**

Get outputs.

Alias: _None_.

Inputs:

* _outputs_  array of  _get\_outputs\_out_  structure as follows:
  * _amount_  - unsigned int;
  * _index_  - unsigned int;

Outputs:

* _outs_  - array of structure  _outkey_  as follows:
  * _height_  - unsigned int; block height of the output
  * _key_  - String; the public key of the output
  * _mask_  - String
  * _txid_  - String; transaction id
  * _unlocked_  - boolean; States if output is locked \(`false`\) or not \(`true`\)
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _untrusted_  - boolean; States if the result is obtained using the bootstrap mode, and is therefore not trusted \(`true`\), or when the daemon is fully synced \(`false`\).

#### **/update**

Update daemon.

Alias: _None_.

Inputs:

* _command_  - String; command to use, either  `check`  or  `download`
* _path_  - String; Optional, path where to download the update.

Outputs:

* _auto\_uri_  - string;
* _hash_  - string;
* _path_  - String; path to download the update
* _status_  - string; General RPC error code. "OK" means everything looks good.
* _update_  - boolean; States if an update is available to download \(`true`\) or not \(`false`\)
* _user\_uri_  - string;
* _version_  - string; Version available for download.

Example:

```text
$ curl -X POST http://127.0.0.1:18081/update -d '{"command":"check"}' -H 'Content-Type: application/json'

{
  "auto_uri": "",
  "hash": "",
  "path": "",
  "status": "OK",
  "update": false,
  "user_uri": "",
  "version": ""
}
```

### Sources:

Reworked from [GetMonero.org](https://ww.getmonero.org/resources/developer-guides/daemon-rpc.html) RPC calls for Oxen under their [copyright license](https://ww.getmonero.org/legal/#copyright).

