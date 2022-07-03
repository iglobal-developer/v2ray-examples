# v2ray-examples

Here are some V2Ray configuration examples for reference. The content keeps pace with the times. Please do not pull the configuration from here, such as automation scripts.

Thanks to KiriKira, author of vTemplate, Silent Rain, and all the developers of Project V.

## Contribution Guidelines

You are welcome to make a template for the configuration you use and submit a PR.

Templates should adhere to the following standards:
- use 4 spaces for indentation
- Square (flower) brackets do not wrap
- Unneeded fields should be removed
- leave only `loglevel` in the `log` part
- For `outbounds`, the client side should have `proxy` and `direct`, the server side should have `direct` and `block`
- `geoip:private` should be routed to `direct` outbound (server configuration routes to `block` outbound) unless it is a template for a specific scenario
- DNS should not be present in the configuration file unless it is a template for a specific scenario
- `uuid` should be left blank and filled by the user.
- `domainStrategy` in `routing` remains default, which is `AsIs`.

### Example

<!-- Here yaml is only used for syntax highlighting, the actual content is json -->
````yaml
{
    "log": {
        "loglevel": "warning"
    },
    "routing": {},
    "inbounds": [],
    "outbounds": []
}
````

### Client

<!-- Here yaml is only used for syntax highlighting, the actual content is json -->
````yaml
{
    "log": {
        "loglevel": "warning"
    },
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            {
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "direct",
                "type": "field"
            }
        ]
    },
    "inbounds": [
        {
            "port": 1080,
            "protocol": "socks",
            "settings": {
                "auth": "noauth",
                "udp": true
            },
            "tag": "socks"
        }
    ],
    "outbounds": [
        {
            "protocol": "vmess",
            "settings": {
                "vnext": [
                    {
                        "users": [
                            {
                                "id": ""
                            }
                        ],
                        "port": 1234,
                        "address": "Your_IP_Address"
                    }
                ]
            }
        },
        {
            "protocol": "freedom",
            "tag": "direct"
        }
    ]
}
````

### Server

<!-- Here yaml is only used for syntax highlighting, the actual content is json -->
````yaml
{
    "log": {
        "loglevel": "warning"
    },
    "routing": {
        "domainStrategy": "AsIs",
        "rules": [
            {
                "ip": [
                    "geoip:private"
                ],
                "outboundTag": "blocked",
                "type": "field"
            }
        ]
    },
    "inbounds": [
        {
            "port": 1234,
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "",
                    }
                ]
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        },
        {
            "protocol": "blackhole",
            "tag": "blocked"
        }
    ]
}
````

## How to choose the configuration that suits you:

![](how-to-choose/how-to-choose-a-v2ray-plan.png)

Additional Notes: <br>
Although Websocket+TLS+Web may be the best solution at this stage, it is definitely not a solution recommended for beginners to try, nor is it the only use of V2Ray. <br>
At the same time, you should understand that the network conditions in each region are different (mainly referring to the QoS level for different protocols). V2Ray is so slow?" such a question.

## at last

have fun!
