## The vfly-core config is different with v2rayU config(xray-core)

- routing don't include the settings field
- inbounds listen use 0.0.0.0 replace with 127.0.0.1

## Template for vfly-core

```json
{
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "protocol": "socks",
      "settings": {
        "udp": false,
        "auth": "noauth"
      },
      "port": "10087"
    },
    {
      "listen": "0.0.0.0",
      "protocol": "http",
      "settings": {
        "timeout": 360
      },
      "port": "10086"
    }
  ],
  "outbounds": [
    {
      "protocol": "vmess",
      "streamSettings": {
        "network": "ws",
        "wsSettings": {
          "path": "/v2ray",
          "headers": {
            "host": ""
          }
        },
        "security": "none"
      },
      "tag": "proxy",
      "settings": {
        "vnext": [
          {
            "address": "...",
            "users": [
              {
                "id": "...",
                "alterId": 2,
                "level": 0,
                "security": "auto"
              }
            ],
            "port": 123
          }
        ]
      }
    },
    {
      "tag": "direct",
      "protocol": "freedom",
      "settings": {
        "domainStrategy": "UseIP",
        "userLevel": 0
      }
    }
  ],
  "routing": {
      "domainStrategy": "IPOnDemand",
      "rules": [
        {
          "type": "field",
          "outboundTag": "direct",
          "domain": [
            "geosite:cn",
            "localhost"
          ]
        },
        {
          "type": "field",
          "ip": [
            "geoip:cn",
            "geoip:private"
          ],
          "outboundTag": "direct"
        }
      ]
    }
}

```