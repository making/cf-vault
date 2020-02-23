# Vault on Cloud Foundry

**This repository is not for production use.**

This vault server uses MySQL as a storage backend.

## Create mysql instance


If you are using [Pivotal Web Services](https://run.pivotal.io):

```
cf create-service cleardb spark vault-db
```

If your are using Pivotal Cloud Foundry:

```
cf create-service p-mysql 100mb-dev vault-db
```

## Deploy Vault

[Download Vault (Linux 64-bit)](https://www.vaultproject.io/downloads.html) and save the binary on this directory.

```
cp manifest-example.yml manifest.yml 
```

Change `name` if needed, then

```
cf push
```

you can see vault has started successfully as following:

```
$ cf logs vault --recent

2020-02-23T19:49:13.61+0900 [CELL/0] OUT Cell 6d545859-b729-4434-8158-8d0ae0e3dd0a successfully created container for instance 2f6f4417-7bec-4292-5c1f-7252
2020-02-23T19:49:14.11+0900 [CELL/0] OUT Downloading droplet...
2020-02-23T19:49:18.60+0900 [CELL/0] OUT Downloaded droplet (45.5M)
2020-02-23T19:49:18.60+0900 [CELL/0] OUT Starting health monitoring of container
2020-02-23T19:49:29.21+0900 [APP/PROC/WEB/0] OUT detected cleardb
2020-02-23T19:49:30.04+0900 [APP/PROC/WEB/0] OUT #### Starting Vault...
2020-02-23T19:49:33.17+0900 [APP/PROC/WEB/0] OUT ==> Vault server configuration:
2020-02-23T19:49:33.20+0900 [APP/PROC/WEB/0] OUT                      Cgo: disabled
2020-02-23T19:49:33.31+0900 [APP/PROC/WEB/0] OUT               Listener 1: tcp (addr: "0.0.0.0:8080", cluster address: "0.0.0.0:8081", max_request_duration: "1m30s", max_request_size: "33554432", tls: "disabled")
2020-02-23T19:49:33.32+0900 [APP/PROC/WEB/0] OUT                Log Level: info
2020-02-23T19:49:33.35+0900 [APP/PROC/WEB/0] OUT                    Mlock: supported: true, enabled: false
2020-02-23T19:49:33.37+0900 [APP/PROC/WEB/0] OUT            Recovery Mode: false
2020-02-23T19:49:33.39+0900 [APP/PROC/WEB/0] OUT                  Storage: mysql (HA disabled)
2020-02-23T19:49:33.40+0900 [APP/PROC/WEB/0] OUT                  Version: Vault v1.3.2
2020-02-23T19:49:33.44+0900 [APP/PROC/WEB/0] OUT ==> Vault server started! Log data will stream in below:
2020-02-23T19:49:33.44+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:49:31.994Z [INFO]  proxy environment: http_proxy= https_proxy= no_proxy=
2020-02-23T19:49:33.44+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:49:33.137Z [WARN]  no `api_addr` value specified in config or in VAULT_API_ADDR; falling back to detection if possible, but this value should be manually set
2020-02-23T19:49:34.78+0900 [CELL/0] OUT Container became healthy
2020-02-23T19:50:30.30+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:30.306Z [ERROR] core: no seal config found, can't determine if legacy or new-style shamir
2020-02-23T19:50:30.46+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:30.460Z [INFO]  core: security barrier not initialized
2020-02-23T19:50:30.65+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:30.654Z [INFO]  core: security barrier initialized: stored=1 shares=5 threshold=3
2020-02-23T19:50:30.83+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:30.830Z [INFO]  core: post-unseal setup starting
2020-02-23T19:50:31.33+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.335Z [INFO]  core: loaded wrapping token key
2020-02-23T19:50:31.33+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.337Z [INFO]  core: successfully setup plugin catalog: plugin-directory=
2020-02-23T19:50:31.38+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.387Z [INFO]  core: no mounts; adding default mount table
2020-02-23T19:50:31.45+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.450Z [INFO]  core: successfully mounted backend: type=cubbyhole path=cubbyhole/
2020-02-23T19:50:31.47+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.460Z [INFO]  core: successfully mounted backend: type=system path=sys/
2020-02-23T19:50:31.51+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:31.510Z [INFO]  core: successfully mounted backend: type=identity path=identity/
2020-02-23T19:50:32.14+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.142Z [INFO]  core: successfully enabled credential backend: type=token path=token/
2020-02-23T19:50:32.14+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.142Z [INFO]  core: restoring leases
2020-02-23T19:50:32.14+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.143Z [INFO]  rollback: starting rollback manager
2020-02-23T19:50:32.18+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.184Z [INFO]  expiration: lease restore complete
2020-02-23T19:50:32.30+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.306Z [INFO]  identity: entities restored
2020-02-23T19:50:32.32+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.324Z [INFO]  identity: groups restored
2020-02-23T19:50:32.43+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.432Z [INFO]  core: post-unseal setup complete
2020-02-23T19:50:32.51+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.514Z [INFO]  core: root token generated
2020-02-23T19:50:32.51+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.514Z [INFO]  core: pre-seal teardown starting
2020-02-23T19:50:32.51+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.514Z [INFO]  rollback: stopping rollback manager
2020-02-23T19:50:32.51+0900 [APP/PROC/WEB/0] ERR 2020-02-23T10:50:32.519Z [INFO]  core: pre-seal teardown complete
```

Now, you can access Vault via like `https://cf-vault.cfapps.io`. Subdomain should be different for your case.

## Initialize vault


```
export VAULT_ADDR=https://<your-sub-domain>.cfapps.io
vault operator init
```

you'll see five unseal keys and root token

```
Unseal Key 1: w6rUcrlOEd4tI0MNtCYxG2uUoGj8wG9euXm4RiHq7BDh
Unseal Key 2: tkGGsCQJeNyORbz2uRyWjCq03kj/OPtGzmM/Bjv9+RTP
Unseal Key 3: 584Sg15Itt8zJpiJOBh+1IVKp56Hv9FiryiK63dztA7C
Unseal Key 4: +ZqZetBMtslvfKfWJ0uCAup51Z5Qx5qobzVjxwqD2rlz
Unseal Key 5: TFYSjAWOjHBARjXBblZuVovtxNnHnSuuHwBthhym8VxL
Initial Root Token: db2c7fae-7162-d09e-7901-66d47360c62f
```


Unseal Vault with three of five unseal keys, for example:

```
vault operator unseal w6rUcrlOEd4tI0MNtCYxG2uUoGj8wG9euXm4RiHq7BDh
vault operator unseal tkGGsCQJeNyORbz2uRyWjCq03kj/OPtGzmM/Bjv9+RTP
vault operator unseal 584Sg15Itt8zJpiJOBh+1IVKp56Hv9FiryiK63dztA7C
```

Authenticate with the root token, for example:

```
vault login db2c7fae-7162-d09e-7901-66d47360c62f
```

Finally, you can read and write Valut :)

```
vault secrets enable -path=kv kv
vault kv put kv/hello target=world
```

## Unseal when restarting

Because Vault seals when it restarts, you need to unseal automatically in order to keep Vault available in CF environment.

If you set environment variables `VAULT_UNSEAL_KEY1`, `VAULT_UNSEAL_KEY2` and `VAULT_UNSEAL_KEY3`, Vaule will be unsealed at the start up

For example:

```
cf set-env vault VAULT_UNSEAL_KEY1 w6rUcrlOEd4tI0MNtCYxG2uUoGj8wG9euXm4RiHq7BDh
cf set-env vault VAULT_UNSEAL_KEY2 tkGGsCQJeNyORbz2uRyWjCq03kj/OPtGzmM/Bjv9+RTP
cf set-env vault VAULT_UNSEAL_KEY3 584Sg15Itt8zJpiJOBh+1IVKp56Hv9FiryiK63dztA7
```

Even if you do `cf restart vault` or `cf restage vault`, `vault` will be available.
Of course you can set these variables in your `manifest.yml`.

Note that this way is [not recommended](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) by Hashicorp.

## Deploy service broker

You can also deploy [Vault Service Broker](https://github.com/hashicorp/cf-vault-service-broker) prodivedby Hashicorp easily :)
