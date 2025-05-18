# Install MongoDB 8.0 and free5GC WebUI
The process for the following OS is shown here.

- Ubuntu 22.04
- Ubuntu 24.04

---

### [Sample Configurations and Miscellaneous for Mobile Network](https://github.com/s5uishida/sample_config_misc_for_mobile_network)

---

<a id="toc"></a>

## Table of Contents

- [Install MongoDB 8.0](#install_mongodb)
- [Install free5GC WebUI](#install_webui)
- [Changelog (summary)](#changelog)

---
<a id="install_mongodb"></a>

## Install MongoDB 8.0

**Note. MongoDB v4.4.19 and later will not run on CPUs that do not support AVX instruction.
In this case, it is necessary to downgrade it to v4.4.18.
For reference, I wrote the steps to install v4.4.18 on Ubuntu 20.04 on Raspberry Pi 4B [here](https://github.com/s5uishida/install_mongodb_on_ubuntu_for_rp4b).**

```
# apt install gnupg curl
# curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor
# echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/8.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-8.0.list
# apt update
# apt install -y mongodb-org
```
```
# systemctl enable mongod
# systemctl start mongod
```

<a id="install_webui"></a>

## Install free5GC WebUI

It is assumed that MongoDB 8.0 and [Go](https://free5gc.org/guide/3-install-free5gc/) has been installed already.  
First, install Node.js, see [here](https://github.com/nodesource/distributions).
```
# apt install -y ca-certificates curl gnupg
# mkdir -p /etc/apt/keyrings
# curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /etc/apt/keyrings/nodesource.gpg
# echo "deb [signed-by=/etc/apt/keyrings/nodesource.gpg] https://deb.nodesource.com/node_22.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list
# apt update
# apt install -y nodejs
```
Next, setup yarn automatically.
```
# corepack enable
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
And, login to Web console as follows and register subscriber information.
```
http://<IP address of Web console>:5000/
username: admin
password: free5gc
```
---
<a id="changelog"></a>

## Changelog (summary)

- [2025.04.10] Updated MongoDB version to 8.0.
- [2024.05.11] Initial release.
