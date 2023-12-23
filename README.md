# X-UI
**An advanced web panel, built on Xray Core, supports multiple protocols, languages, and features a sophisticated routing interface**

![](https://img.shields.io/github/v/release/alireza0/x-ui.svg)
![](https://img.shields.io/docker/pulls/alireza7/x-ui.svg)
[![Go Report Card](https://goreportcard.com/badge/github.com/alireza0/x-ui)](https://goreportcard.com/report/github.com/alireza0/x-ui)
[![Downloads](https://img.shields.io/github/downloads/alireza0/x-ui/total.svg)](https://img.shields.io/github/downloads/alireza0/x-ui/total.svg)
[![License](https://img.shields.io/badge/license-GPL%20V3-blue.svg?longCache=true)](https://www.gnu.org/licenses/gpl-3.0.en.html)

> **Disclaimer: This project is only for personal learning and communication, please do not use it for illegal purposes, please do not use it in a production environment**


**If you think this project is helpful to you, you may wish to give a** :star2:

**Buy Me a Coffee :**

- USDT Tron (TRC20): `TYTq73Gj6dJ67qe58JVPD9zpjW2cc9XgVz`
- Tezos (XTZ):
`tz2Wnh2SsY1eezXrcLChu6idWpgdHzUFQcts`


## Quick Look
| Features                               |      Enable?       |
| -------------------------------------- | :----------------: |
| Multi-Protocol                         | :heavy_check_mark: |
| Multi-Language                         | :heavy_check_mark: |
| Multi-Client Inbounds                  | :heavy_check_mark: |
| Advanced Traffic Routing               | :heavy_check_mark: |
| REST API                               | :heavy_check_mark: |
|Display Online Clients                  | :heavy_check_mark: |
| Manage Clients Traffic Data & Expiry Date| :heavy_check_mark: |
| Applies Expiry Date Based on First Usage | :heavy_check_mark: |
| Telegram Bot (admin + clients)         | :heavy_check_mark: |
| Database Backup using Telegram Bot     | :heavy_check_mark: |
| Subscription Link + Client Info        | :heavy_check_mark: |
| Search in Deep                         | :heavy_check_mark: |
| Dark/Light Theme                       | :heavy_check_mark: |

  
## Install & Upgrade to Latest Version

```sh
bash <(curl -Ls https://raw.githubusercontent.com/alireza0/x-ui/master/install.sh)
```

## Install Custom Version

To install your desired version, add the version to the end of the installation command. For example, for version `0.5.2`:

```sh
bash <(curl -Ls https://raw.githubusercontent.com/alireza0/x-ui/master/install.sh) 0.5.2
```

## Manual Install & Upgrade

1. First download the latest compressed package from https://github.com/alireza0/x-ui/releases, generally choose Architecture `amd64`
2. Then upload the compressed package to the server's `/root/` directory and login to the server with user `root` 

> If your server CPU architecture is not `amd64` replace it with the appropriate architecture

```sh
ARCH=$(uname -m)
[[ "${ARCH}" == "s390x" ]] && XUI_ARCH="s390x" || [[ "${ARCH}" == "aarch64" || "${ARCH}" == "arm64" ]] && XUI_ARCH="arm64" || XUI_ARCH="amd64"
cd /root/
rm x-ui/ /usr/local/x-ui/ /usr/bin/x-ui -rf
tar zxvf x-ui-linux-${XUI_ARCH}.tar.gz
chmod +x x-ui/x-ui x-ui/bin/xray-linux-* x-ui/x-ui.sh
cp x-ui/x-ui.sh /usr/bin/x-ui
cp -f x-ui/x-ui.service /etc/systemd/system/
mv x-ui/ /usr/local/
systemctl daemon-reload
systemctl enable x-ui
systemctl restart x-ui
```

## Install Using Docker

1. Install docker

```shell
curl -fsSL https://get.docker.com | sh
```

2. Install X-UI

```shell
mkdir x-ui && cd x-ui
docker run -itd \
    -p 54321:54321 -p 443:443 -p 80:80 \
    -e XRAY_VMESS_AEAD_FORCED=false \
    -v $PWD/db/:/etc/x-ui/ \
    -v $PWD/cert/:/root/cert/ \
    --name x-ui --restart=unless-stopped \
    alireza7/x-ui:latest
```

> Build your own image

```shell
docker build -t x-ui .
```

## Languages

- English
- Chinese
- Farsi
- Russian
- Vietnamese

## Features

- Supported protocols: VLESS, VMess, Trojan, Shadowsocks, Dokodemo-door, Socks, HTTP
- Supports XTLS Protocols (Vision, REALITY)
- Supports an advanced JSON editor interface for Xray-Core configuration 
- Supports an advanced interface for routing traffic, including Reverse and Transparent proxy, Multi-Domain, Multi-Certificate, and Multi-Port per inbound
- Supports Multi-User per inbound
- Supports traffic data limits and expiry dates per client/inbound
- Supports system status monitoring
- Supports deep database search
- Displays live traffic statistics
- Displays online clients
- Displays clients with expired dates or exceeded traffic limits
- Supports subscription service with (multi) link
- Supports importing and exporting databases
- Supports one-click SSL certificate application and automatic renewal
- Supports HTTPS for admin web panel and subscription service (self-provided domain name + SSL certificate)
- Supports Dark/Light theme UI

## Recommended OS

- CentOS 8+
- Ubuntu 20+
- Debian 10+
- Fedora 36+

## Screenshots

![inbounds](./media/inbounds.png)
![Dark inbounds](./media/inbounds-dark.png)
![outbounds](./media/outbounds.png)
![rules](./media/rules.png)


## API Routes

<details>
  <summary>Click for details</summary>

### Usage

- `/login` with `PUSH` user data: `{username: '', password: ''}` for login
- `/xui/API/inbounds` base for following actions:

| Method | Path                               | Action                                    |
| :----: | ---------------------------------  | ----------------------------------------- |
| `GET`  | `"/"`                              | Get all inbounds                          |
| `GET`  | `"/get/:id"`                       | Get inbound with inbound.id               |
| `GET`  | `"/createbackup"`                  | Telegram bot sends backup to admins       |
| `POST` | `"/add"`                           | Add inbound                               |
| `POST` | `"/del/:id"`                       | Delete inbound                            |
| `POST` | `"/update/:id"`                    | Update inbound                            |
| `POST` | `"/addClient/"`                    | Add client to inbound                     |
| `POST` | `"/:id/delClient/:clientId"`       | Delete client by clientId\*               |
| `POST` | `"/updateClient/:clientId"`        | Update client by clientId\*               |
| `GET`  | `"/getClientTraffics/:email"`      | Get client's traffic                      |
| `POST` | `"/:id/resetClientTraffic/:email"` | Reset client's traffic                    |
| `POST` | `"/resetAllTraffics"`              | Reset traffics of all inbounds            |
| `POST` | `"/resetAllClientTraffics/:id"`    | Reset inbound clients traffics (-1: all)  |
| `POST` | `"/delDepletedClients/:id"`        | Delete inbound depleted clients (-1: all) |
| `POST` | `"/onlines"`                       | Get online users ( list of emails )       |

\*- The field `clientId` should be filled by:

- `client.id` for VMess and VLESS
- `client.password` for Trojan
- `client.email` for Shadowsocks

</details>

## Environment Variables

<details>
  <summary>Click for details</summary>

### Usage

| Variable       |                      Type                      | Default       |
| -------------- | :--------------------------------------------: | :------------ |
| XUI_LOG_LEVEL  | `"debug"` \| `"info"` \| `"warn"` \| `"error"` | `"info"`      |
| XUI_DEBUG      |                   `boolean`                    | `false`       |
| XUI_BIN_FOLDER |                    `string`                    | `"bin"`       |
| XUI_DB_FOLDER  |                    `string`                    | `"/etc/x-ui"` |

</details>

## SSL Certificate

<details>
  <summary>Click for details</summary>

### Certbot

```bash
snap install core; snap refresh core
snap install --classic certbot
ln -s /snap/bin/certbot /usr/bin/certbot

certbot certonly --standalone --register-unsafely-without-email --non-interactive --agree-tos -d <Your Domain Name>
```

</details>

## Telegram Bot

<details>
  <summary>Click for details</summary>

### Usage

X-UI panel supports daily traffic notifications, panel login reminders, and other functions through the Telegram (Tg) robot. To use the Tg robot, you need to follow the specific application tutorial. Refer to the [blog](https://coderfan.net/how-to-use-telegram-bot-to-alarm-you-when-someone-login-into-your-vps.html)
to set the robot-related parameters in the panel background, including:

- Tg robot Token
- Tg robot ChatId
- Tg robot cycle runtime, in crontab syntax
- Tg robot Expiration threshold
- Tg robot Traffic threshold
- Tg robot Enable send backup in cycle runtime
- Tg robot Enable CPU usage alarm threshold

Reference syntax:

- 30 \* \* \* \* \* //Notify at the 30s of each point
- 0 \*/10 \* \* \* \* //Notify at the first second of each 10 minutes
- @hourly // hourly notification
- @daily // Daily notification (00:00 in the morning)
- @every 8h // notify every 8 hours

### Features

- Report periodic
- Login notification
- CPU threshold notification
- Threshold for Expiration time and Traffic to report in advance
- Support client report menu if client's telegram ID or UserName added to the user's configurations
- Support telegram traffic report searched with UUID (VMESS/VLESS) or Password (TROJAN) - anonymously
- Menu based bot
- Search client by email ( only admin )
- Check all inbounds
- Check server status
- Check depleted users
- Receive backup by request and in periodic reports
- Multi language bot
</details>

## Troubleshoots

<details>
  <summary>Click for details</summary>

### Enable Traffic Usage

Please be aware that if you upgrade from an older X-UI version or other forks, data traffic usage for users may not work by default. It is recommended to follow the steps below for enabling:

1. Find this section in config file

```json
 "policy": {
    "system": {
```

2. Add below section just after ` "policy": {` :

```json
    "levels": {
      "0": {
        "statsUserUplink": true,
        "statsUserDownlink": true
      }
    },
```

- The final output is like:

```json
  "policy": {
    "levels": {
      "0": {
        "statsUserUplink": true,
        "statsUserDownlink": true
      }
    },

    "system": {
      "statsInboundDownlink": true,
      "statsInboundUplink": true
    }
  },
  "routing": {
```

3. Save and restart panel

</details>

## A Special Thanks to

- [HexaSoftwareTech](https://github.com/HexaSoftwareTech/)
- [MHSanaei](https://github.com/MHSanaei)

## Acknowledgment

- [Loyalsoldier](https://github.com/Loyalsoldier/v2ray-rules-dat) (License: **GPL-3.0**): _The enhanced version of V2Ray routing rule._
- [Iran v2ray rules](https://github.com/chocolate4u/Iran-v2ray-rules) (License: **GPL-3.0**): _Enhanced v2ray/xray and v2ray/xray-clients routing rules with built-in Iranian domains and a focus on security and adblocking._

## Stargazers over Time

[![Stargazers over time](https://starchart.cc/alireza0/x-ui.svg)](https://starchart.cc/alireza0/x-ui)
