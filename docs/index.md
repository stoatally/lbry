# LBRY JSON-RPC API Documentation

## blob_announce_all

```text
Announce all blobs to the DHT

Args:
    None
Returns:
    (str) Success/fail message
```

## blob_delete

```text
Delete a blob

Args:
    'blob_hash': (str) hash of blob to get
Returns:
    (str) Success/fail message
```

## blob_get

```text
Download and return a blob

Args:
    'blob_hash': (str) blob hash of blob to get
    'timeout'(optional): (int) timeout in number of seconds
    'encoding'(optional): (str) by default no attempt at decoding is made,
                         can be set to one of the following decoders:
                         'json'
    'payment_rate_manager'(optional): if not given the default payment rate manager
                                     will be used. supported alternative rate managers:
                                     'only-free'

Returns
    (str) Success/Fail message or (dict) decoded data
```

## blob_list

```text
Returns blob hashes. If not given filters, returns all blobs known by the blob manager

Args:
    'uri' (optional): (str) filter by blobs in stream for winning claim
    'stream_hash' (optional): (str) filter by blobs in given stream hash
    'sd_hash' (optional): (str) filter by blobs in given sd hash
    'needed' (optional): (bool) only return needed blobs
    'finished' (optional): (bool) only return finished blobs
    'page_size' (optional): (int) limit number of results returned
    'page' (optional): (int) filter to page x of [page_size] results
Returns:
    (list) List of blob hashes
```

## blob_reflect_all

```text
Reflects all saved blobs

Args:
    None
Returns:
    (bool) true if successful
```

## block_show

```text
Get contents of a block

Args:
    'blockhash': (str) hash of the block to look up
Returns:
    (dict) Requested block
```

## claim_abandon

```text
Abandon a name and reclaim credits from the claim

Args:
    'txid': (str) txid of claim
    'nout': (int) nout of claim
Return:
    (dict) Dictionary containing result of the claim
    {
        txid : (str) txid of resulting transaction
        fee : (float) fee paid for the transaction
    }
```

## claim_list

```text
Get claims for a name

Args:
    'name': (str) search for claims on this name
Returns
    (dict) State of claims assigned for the name
    {
        'claims': (list) list of claims for the name
        [
            {
            'amount': (float) amount assigned to the claim
            'effective_amount': (float) total amount assigned to the claim,
                                including supports
            'claim_id': (str) claim ID of the claim
            'height': (int) height of block containing the claim
            'txid': (str) txid of the claim
            'nout': (int) nout of the claim
            'supports': (list) a list of supports attached to the claim
            'value': (str) the value of the claim
            },
        ]
        'supports_without_claims': (list) supports without any claims attached to them
        'last_takeover_height': (int) the height of last takeover for the name
    }
```

## claim_list_mine

```text
List my name claims

Args:
    None
Returns
    (list) List of name claims owned by user
    [
        {
            'address': (str) address that owns the claim
            'amount': (float) amount assigned to the claim
            'blocks_to_expiration': (int) number of blocks until it expires
            'category': (str) "claim", "update" , or "support"
            'claim_id': (str) claim ID of the claim
            'confirmations': (int) number of blocks of confirmations for the claim
            'expiration_height': (int) the block height which the claim will expire
            'expired': (bool) true if expired, false otherwise
            'height': (int) height of the block containing the claim
            'is_spent': (bool) true if claim is abandoned, false otherwise
            'name': (str) name of the claim
            'txid': (str) txid of the cliam
            'nout': (int) nout of the claim
            'value': (str) value of the claim
        },
   ]
```

## claim_new_support

```text
Support a name claim

Args:
    'name': (str) name
    'claim_id': (str) claim ID of claim to support
    'amount': (float) amount to support by
Return:
    (dict) Dictionary containing result of the claim
    {
        txid : (str) txid of resulting support claim
        nout : (int) nout of the resulting support claim
        fee : (float) fee paid for the transaction
    }
```

## claim_show

```text
Resolve claim info from a LBRY name

Args:
    'name': (str) name to look up, do not include lbry:// prefix
    'txid'(optional): (str) if specified, look for claim with this txid
    'nout'(optional): (int) if specified, look for claim with this nout
Returns:
    (dict) Dictionary contaning claim info, (bool) false if claim is not
        resolvable

    {
        'txid': (str) txid of claim
        'nout': (int) nout of claim
        'amount': (float) amount of claim
        'value': (str) value of claim
        'height' : (int) height of claim takeover
        'claim_id': (str) claim ID of claim
        'supports': (list) list of supports associated with claim
    }
```

## commands

```text
Return a list of available commands

Returns:
    (list) list of available commands
```

## daemon_stop

```text
Stop lbrynet-daemon

Returns:
    (string) Shutdown message
```

## descriptor_get

```text
Download and return a sd blob

Args:
    'sd_hash': (str) hash of sd blob
    'timeout'(optional): (int) timeout in number of seconds
    'payment_rate_manager'(optional): (str) if not given the default payment rate manager
                                     will be used. supported alternative rate managers:
                                     only-free

Returns
    (str) Success/Fail message or (dict) decoded data
```

## file_delete

```text
Delete a lbry file

Args:
    'name' (optional): (str) delete file by lbry name,
    'sd_hash' (optional): (str) delete file by sd hash,
    'file_name' (optional): (str) delete file by the name in the downloads folder,
    'stream_hash' (optional): (str) delete file by stream hash,
    'claim_id' (optional): (str) delete file by claim ID,
    'outpoint' (optional): (str) delete file by claim outpoint,
    'rowid': (optional): (int) delete file by rowid in the file manager
    'delete_target_file' (optional): (bool) delete file from downloads folder,
                                    defaults to true if false only the blobs and
                                    db entries will be deleted
Returns:
    (bool) true if deletion was successful
```

## file_list

```text
List files limited by optional filters

Args:
    'name' (optional): (str) filter files by lbry name,
    'sd_hash' (optional): (str) filter files by sd hash,
    'file_name' (optional): (str) filter files by the name in the downloads folder,
    'stream_hash' (optional): (str) filter files by stream hash,
    'claim_id' (optional): (str) filter files by claim id,
    'outpoint' (optional): (str) filter files by claim outpoint,
    'rowid' (optional): (int) filter files by internal row id,
    'full_status': (optional): (bool) if true populate the 'message' and 'size' fields

Returns:
    (list) List of files

    [
        {
            'completed': (bool) true if download is completed,
            'file_name': (str) name of file,
            'download_directory': (str) download directory,
            'points_paid': (float) credit paid to download file,
            'stopped': (bool) true if download is stopped,
            'stream_hash': (str) stream hash of file,
            'stream_name': (str) stream name ,
            'suggested_file_name': (str) suggested file name,
            'sd_hash': (str) sd hash of file,
            'name': (str) name claim attached to file
            'outpoint': (str) claim outpoint attached to file
            'claim_id': (str) claim ID attached to file,
            'download_path': (str) download path of file,
            'mime_type': (str) mime type of file,
            'key': (str) key attached to file,
            'total_bytes': (int) file size in bytes, None if full_status is false
            'written_bytes': (int) written size in bytes
            'message': (str), None if full_status is false
            'metadata': (dict) Metadata dictionary
        },
    ]
```

## file_set_status

```text
Start or stop downloading a file

Args:
    'status': (str) "start" or "stop"
    'name' (optional): (str) start file by lbry name,
    'sd_hash' (optional): (str) start file by the hash in the name claim,
    'file_name' (optional): (str) start file by its name in the downloads folder,
Returns:
    (str) Confirmation message
```

## get

```text
Download stream from a LBRY name.

Args:
    'name': (str) name to download
    'file_name'(optional): (str) a user specified name for the downloaded file
    'stream_info'(optional): (str) specified stream info overrides name
    'timeout'(optional): (int) download timeout in number of seconds
    'download_directory'(optional): (str) path to directory where file will be saved
    'wait_for_write'(optional): (bool)  defaults to True. When set, waits for the file to
        only start to be written before returning any results.
Returns:
    (dict) Dictionary contaning information about the stream

    {
        'completed': (bool) true if download is completed,
        'file_name': (str) name of file,
        'download_directory': (str) download directory,
        'points_paid': (float) credit paid to download file,
        'stopped': (bool) true if download is stopped,
        'stream_hash': (str) stream hash of file,
        'stream_name': (str) stream name ,
        'suggested_file_name': (str) suggested file name,
        'sd_hash': (str) sd hash of file,
        'name': (str) name claim attached to file
        'outpoint': (str) claim outpoint attached to file
        'claim_id': (str) claim ID attached to file,
        'download_path': (str) download path of file,
        'mime_type': (str) mime type of file,
        'key': (str) key attached to file,
        'total_bytes': (int) file size in bytes, None if full_status is false
        'written_bytes': (int) written size in bytes
        'message': (str), None if full_status is false
        'metadata': (dict) Metadata dictionary
    }
```

## get_availability

```text
Get stream availability for a winning claim

Args:
    'name' : (str) lbry name
    'sd_timeout' (optional): (int) sd blob download timeout
    'peer_timeout' (optional): (int) how long to look for peers

Returns:
    (float) Peers per blob / total blobs
```

## help

```text
Return a useful message for an API command

Args:
    'command'(optional): (str) command to retrieve documentation for
Returns:
    (str) if given a command, returns documentation about that command
    otherwise returns general help message
```

## peer_list

```text
Get peers for blob hash

Args:
    'blob_hash': (str) blob hash
    'timeout'(optional): (int) peer search timeout in seconds
Returns:
    (list) List of contacts
```

## publish

```text
Make a new name claim and publish associated data to lbrynet,
update over existing claim if user already has a claim for name.

Fields required in the final Metadata are:
    'title'
    'description'
    'author'
    'language'
    'license',
    'nsfw'

Metadata can be set by either using the metadata argument or by setting individual arguments
fee, title, description, author, language, license, license_url, thumbnail, preview, nsfw,
or sources. Individual arguments will overwrite the fields specified in metadata argument.

Args:
    'name': (str) name to be claimed
    'bid': (float) amount of credits to commit in this claim,
    'metadata'(optional): (dict) Metadata to associate with the claim.
    'file_path'(optional): (str) path to file to be associated with name. If provided,
                            a lbry stream of this file will be used in 'sources'.
                            If no path is given but a metadata dict is provided, the source
                            from the given metadata will be used.
    'fee'(optional): (dict) Dictionary representing key fee to download content:
                      {currency_symbol: {'amount': float, 'address': str, optional}}
                      supported currencies: LBC, USD, BTC
                      If an address is not provided a new one will be automatically
                      generated. Default fee is zero.
    'title'(optional): (str) title of the file
    'description'(optional): (str) description of the file
    'author'(optional): (str) author of the file
    'language'(optional): (str), language code
    'license'(optional): (str) license for the file
    'license_url'(optional): (str) URL to license
    'thumbnail'(optional): (str) thumbnail URL for the file
    'preview'(optional): (str) preview URL for the file
    'nsfw'(optional): (bool) True if not safe for work
    'sources'(optional): (dict){'lbry_sd_hash':sd_hash} specifies sd hash of file

Returns:
    (dict) Dictionary containing result of the claim
    {
        'tx' : (str) hex encoded transaction
        'txid' : (str) txid of resulting claim
        'nout' : (int) nout of the resulting claim
        'fee' : (float) fee paid for the claim transaction
        'claim_id' : (str) claim ID of the resulting claim
    }
```

## reflect

```text
Reflect a stream

Args:
    'sd_hash': (str) sd_hash of lbry file
Returns:
    (bool) true if successful
```

## report_bug

```text
Report a bug to slack

Args:
    'message': (str) message to send
Returns:
    (bool) true if successful
```

## resolve_name

```text
Resolve stream info from a LBRY name

Args:
    'name': (str) name to look up, do not include lbry:// prefix
Returns:
    (dict) Metadata dictionary from name claim, None if the name is not
            resolvable
```

## send_amount_to_address

```text
Send credits to an address

Args:
    'amount': (float) the amount to send
    'address': (str) the address of the recipient in base58
Returns:
    (bool) true if payment successfully scheduled
```

## settings_get

```text
Get daemon settings

Returns:
    (dict) Dictionary of daemon settings
    See ADJUSTABLE_SETTINGS in lbrynet/conf.py for full list of settings
```

## settings_set

```text
Set daemon settings

Args:
    'run_on_startup': (bool) currently not supported
    'data_rate': (float) data rate,
    'max_key_fee': (float) maximum key fee,
    'download_directory': (str) path of where files are downloaded,
    'max_upload': (float), currently not supported
    'max_download': (float), currently not supported
    'download_timeout': (int) download timeout in seconds
    'search_timeout': (float) search timeout in seconds
    'cache_time': (int) cache timeout in seconds
Returns:
    (dict) Updated dictionary of daemon settings
```

## status

```text
Return daemon status

Args:
    'session_status' (optional): (bool) true to return session status,
        default is false
Returns:
    (dict) Daemon status dictionary
```

## stream_cost_estimate

```text
Get estimated cost for a lbry stream

Args:
    'name': (str) lbry name
    'size' (optional): (int) stream size, in bytes. if provided an sd blob
                        won't be downloaded.
Returns:
    (float) Estimated cost in lbry credits
```

## transaction_list

```text
List transactions belonging to wallet

Args:
    None
Returns:
    (list) List of transactions
```

## transaction_show

```text
Get a decoded transaction from a txid

Args:
    'txid': (str) txid of transaction
Returns:
    (dict) JSON formatted transaction
```

## version

```text
Get lbry version information

Args:
    None
Returns:
    (dict) Dictionary of lbry version information
    {
        'build': (str) build type (e.g. "dev", "rc", "release"),
        'ip': (str) remote ip, if available,
        'lbrynet_update_available': (bool) whether there's an update available,
        'lbrynet_version': (str) lbrynet_version,
        'lbryum_version': (str) lbryum_version,
        'os_release': (str) os release string
        'os_system': (str) os name
        'platform': (str) platform string
        'processor': (str) processor type,
        'python_version': (str) python version,
        'remote_lbrynet': (str) most recent lbrynet version available from github
    }
```

## wallet_balance

```text
Return the balance of the wallet

Returns:
    (float) amount of lbry credits in wallet
```

## wallet_is_address_mine

```text
Checks if an address is associated with the current wallet.

Args:
    'address': (str) address to check in base58
Returns:
    (bool) true, if address is associated with current wallet
```

## wallet_list

```text
List wallet addresses

Args:
    None
Returns:
    List of wallet addresses
```

## wallet_new_address

```text
Generate a new wallet address

Args:
    None
Returns:
    (str) New wallet address in base58
```

## wallet_public_key

```text
Get public key from wallet address

Args:
    'address': (str) wallet address in base58
Returns:
    (list) list of public keys associated with address.
        Could contain more than one public key if multisig.
```

## wallet_unused_address

```text
Return an address containing no balance, will create
a new address if there is none.

Args:
    None
Returns:
    (str) Unused wallet address in base58
```

