# Install MongoDB 7.0 and free5GC WebUI
The process for the following OS is shown here.

- Ubuntu 24.04

---

### [Sample Configurations and Miscellaneous for Mobile Network](https://github.com/s5uishida/sample_config_misc_for_mobile_network)

---

<a id="toc"></a>

## Table of Contents

- [Install MongoDB 7.0](#install_mongodb)
- [Install free5GC WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<a id="install_mongodb"></a>

## Install MongoDB 7.0

**Note. MongoDB v4.4.19 and later will not run on CPUs that do not support AVX instruction.
In this case, it is necessary to downgrade it to v4.4.18.
For reference, I wrote the steps to install v4.4.18 on Ubuntu 20.04 on Raspberry Pi 4B [here](https://github.com/s5uishida/install_mongodb_on_ubuntu_for_rp4b).**

```
# apt update
# apt install gnupg wget
# wget -qO - https://www.mongodb.org/static/pgp/server-7.0.asc | gpg --dearmor -o /etc/apt/trusted.gpg.d/mongodb-7.gpg
# echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-7.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<a id="install_webui"></a>

## Install free5GC WebUI

It is assumed that MongoDB 7.0 and [Go](https://free5gc.org/guide/3-install-free5gc/) has been installed already.

First, install Yarn.
```
# apt install wget
# wget -qO - https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor -o /etc/apt/trusted.gpg.d/yarn.gpg
# echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
# apt update
# apt install -y yarn
```
Next, install Node.js, see [here](https://github.com/nodesource/distributions).
```
# apt install -y ca-certificates curl gnupg
# mkdir -p /etc/apt/keyrings
# curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
# echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_20.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
# apt update
# apt install -y nodejs
```
Then, install free5GC WebUI.
```
# git clone --recursive -j `nproc` https://github.com/free5gc/free5gc.git
# cd free5gc/webconsole
# git checkout main
# cd ..
# make webconsole
```
Run the WebUI.
```
# cd webconsole
# bin/webconsole
...
[GIN-debug] Listening and serving HTTP on 192.168.0.141:5000
2024-05-12T01:22:33.133302640+09:00 [INFO][category:FTPServer][component:Billing] Starting...
```
If necessary, set the IP address and port to bind as follows (ex. `192.168.0.141:5000`).

`free5gc/webconsole/config/webuicfg.yaml`
```diff
--- webuicfg.yaml.orig  2024-05-12 00:37:04.337977658 +0900
+++ webuicfg.yaml       2024-05-12 00:50:30.617768083 +0900
@@ -9,7 +9,7 @@
   nrfUri: http://127.0.0.10:8000 # a valid URI of NRF
   webServer:
     scheme: http
-    ipv4Address: 0.0.0.0
+    ipv4Address: 192.168.0.141
     port: 5000
   billingServer:
     enable: true
```

---
<a id="changelog"></a>

## Changelog (summary)

- [2024.05.11] Initial release.
