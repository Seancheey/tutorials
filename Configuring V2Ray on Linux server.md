Configuring V2Ray on Linux server
===

***Note:*** This is just a quick note of step-to-step instructions to install and configure v2ray. To learn more about it, check http://v2ray.com for extra information.

1: Install V2Ray with official shell script:
---
```shell
bash <(curl -L -s https://install.direct/go.sh)
```

2: Edit /etc/v2ray/config.json:
---
Use vi or any other editor to open config file
```
vi /etc/v2ray/config.json
```
Change only the inbound configurations. You can choose your own port or UUID if you like. You can go to https://www.uuidgenerator.net to generate a random UUID, this will also be your client login ID.


```
"inbound": {
  "port": 37101,
  "protocol": "vmess",
  "settings": {
    "clients": [
      {
        "id": "562e6f0c-2bfb-4d94-b688-93256270dadf",
        "level": 1,
        "alterId": 64
      }
    ]
  }
}
```
(Or you can choose any of these UUID for convenience)
```
59b9ca30-0693-4e5e-bb70-b8e796996421
df006ffa-2a53-4037-8dfa-9765609593d0
5c57851a-d05d-4574-a0c5-cdefd814ce9a
a8ddba71-09d3-4088-b7b6-b79e96ae87ac
41a3b976-f956-4179-a87e-09d32352549c
74da7222-7e4f-4a46-b9c1-3280c31e428e
f6752c1d-51f3-41f4-8397-23f9e08db1d0
9a80d984-6918-4a9e-9a7c-e56a76b2484e
b0753067-513c-4654-b9ff-b7cdc5b7dfbf
c9ce0fc3-e081-4b10-b6e7-2b9044a915df
```

3: Start the V2Ray service
---
```shell
service v2ray start
service v2ray status
```
