# Vault on Cloud Foundry

** This repository is not for production use. **

This vault server uses MySQL as a storage backend.

## Create mysql instance


If you are using [Pivotal Web Services](https://run.pivotal.io):

```
cf create-service cleardb spark vault-db
```

If your are using Pivotal Cloud Foundry:

```
cf create-service 100mb-dev vault-db
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
$ cf logs cf-vault --recent

2017-05-22T23:56:55.26+0900 [API/6]      OUT Created app with guid 49db8210-94cb-4ef9-b76a-f98985ae4748
2017-05-22T23:56:59.25+0900 [API/4]      OUT Updated app with guid 49db8210-94cb-4ef9-b76a-f98985ae4748 ({"route"=>"f235f09f-82e6-419b-b68b-d87aee6e532e", :verb=>"add", :relation=>"routes", :related_guid=>"f235f09f-82e6-419b-b68b-d87aee6e532e"})
2017-05-22T23:57:20.04+0900 [API/3]      OUT Updated app with guid 49db8210-94cb-4ef9-b76a-f98985ae4748 ({"state"=>"STARTED"})
2017-05-22T23:57:20.54+0900 [STG/0]      OUT Downloading binary_buildpack...
2017-05-22T23:57:20.59+0900 [STG/0]      OUT Downloaded binary_buildpack
2017-05-22T23:57:20.59+0900 [STG/0]      OUT Creating container
2017-05-22T23:57:21.66+0900 [STG/0]      OUT Successfully created container
2017-05-22T23:57:21.66+0900 [STG/0]      OUT Downloading app package...
2017-05-22T23:57:23.51+0900 [STG/0]      OUT Downloaded app package (13.5M)
2017-05-22T23:57:23.74+0900 [STG/0]      OUT -------> Buildpack version 1.0.13
2017-05-22T23:57:27.32+0900 [STG/0]      OUT Exit status 0
2017-05-22T23:57:27.32+0900 [STG/0]      OUT Uploading droplet, build artifacts cache...
2017-05-22T23:57:27.33+0900 [STG/0]      OUT Uploading droplet...
2017-05-22T23:57:27.33+0900 [STG/0]      OUT Uploading build artifacts cache...
2017-05-22T23:57:27.43+0900 [STG/0]      OUT Uploaded build artifacts cache (202B)
2017-05-22T23:57:29.77+0900 [STG/0]      OUT Uploaded droplet (13.5M)
2017-05-22T23:57:29.78+0900 [STG/0]      OUT Uploading complete
2017-05-22T23:57:29.85+0900 [STG/0]      OUT Destroying container
2017-05-22T23:57:30.58+0900 [CELL/0]     OUT Creating container
2017-05-22T23:57:30.94+0900 [STG/0]      OUT Successfully destroyed container
2017-05-22T23:57:31.32+0900 [CELL/0]     OUT Successfully created container
2017-05-22T23:57:32.85+0900 [CELL/0]     OUT Starting health monitoring of container
2017-05-22T23:57:32.98+0900 [APP/PROC/WEB/0]OUT detected cleardb
2017-05-22T23:57:33.00+0900 [APP/PROC/WEB/0]OUT #### Starting Vault...
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT ==> Vault server configuration:
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT                      Cgo: disabled
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT               Listener 1: tcp (addr: "0.0.0.0:8080", cluster address: "0.0.0.0:8081", tls: "disabled")
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT                Log Level: info
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT                    Mlock: supported: true, enabled: false
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT                  Storage: mysql
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT                  Version: Vault v0.7.2
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT              Version Sha: d28dd5a018294562dbc9a18c95554d52b5d12390
2017-05-22T23:57:33.10+0900 [APP/PROC/WEB/0]OUT ==> Vault server started! Log data will stream in below:
2017-05-22T23:57:34.95+0900 [CELL/0]     OUT Container became healthy
```

Now, you can access Vault via like `https://cf-vault.cfapps.io`. Subdomain should be different for your case.

## Initialize vault


```
export VAULT_ADDR=https://<your-sub-domain>.cfapps.io
vault init
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

Authenticate with the root token, for example:

```
vault auth db2c7fae-7162-d09e-7901-66d47360c62f
```

Unseal Vault with three of five unseal keys, for example:

```
vault unseal w6rUcrlOEd4tI0MNtCYxG2uUoGj8wG9euXm4RiHq7BDh
vault unseal tkGGsCQJeNyORbz2uRyWjCq03kj/OPtGzmM/Bjv9+RTP
vault unseal 584Sg15Itt8zJpiJOBh+1IVKp56Hv9FiryiK63dztA7C
```

Finally, you can read and write Valut :)

```
$ vault write secret/hello vaule=world
Success! Data written to: secret/hello

$ vault read secret/hello
Key             	Value
---             	-----
refresh_interval	768h0m0s
vaule           	world
```

## Unseal when restarting

Because Vault seals when it restarts. You need to unseal automatically to keep Vault available in CF environment.

If you set environment variables `VAULT_TOKEN` and `VAULT_UNSEAL_KEY1`, `VAULT_UNSEAL_KEY2`, `VAULT_UNSEAL_KEY3` at the start up.

For example:

```
cf set-env cf-vault VAULT_TOKEN db2c7fae-7162-d09e-7901-66d47360c62f
cf set-env cf-vault VAULT_UNSEAL_KEY1 w6rUcrlOEd4tI0MNtCYxG2uUoGj8wG9euXm4RiHq7BDh
cf set-env cf-vault VAULT_UNSEAL_KEY2 tkGGsCQJeNyORbz2uRyWjCq03kj/OPtGzmM/Bjv9+RTP
cf set-env cf-vault VAULT_UNSEAL_KEY3 584Sg15Itt8zJpiJOBh+1IVKp56Hv9FiryiK63dztA7
```

you can `cf restart cf-vault` nad `cf-vault` will be available.
Of course you can set these variables in your `manifest.yml`.

Note that this way is [not recommended](https://www.vaultproject.io/docs/concepts/seal.html#unsealing) by Hashicorp.