# [Hysteria 2](https://github.com/apernet/hysteria/tree/wip-hy2) Installation guide

## Server side

### install

1. [Download program](https://github.com/apernet/hysteria/actions/workflows/dev-build-hy2.yml)

- 将 **hysteria-linux-amd64** Changed its name to **hysteria**，Upload it to **/usr/local/bin** Table of contents，Execute the following command

```
chmod +x /usr/local/bin/hysteria
```

- [Compile the program](https://github.com/chika0801/hysteria-install/blob/main/compile_hysteria.md)

2. Download configuration

```
curl -Lo /root/hysteria_config.yaml https://raw.githubusercontent.com/chika0801/hysteria-install/main/config_server.yaml
```

3. Download systemd configuration

```
curl -Lo /etc/systemd/system/hysteria.service https://raw.githubusercontent.com/chika0801/hysteria-install/main/hysteria.service && systemctl daemon-reload
```

4. Upload certificate and private key

- Rename the certificate file to  **fullchain.cer**，Rename the private key file to **private.key**，Upload them to **/root** Table of contents

5. Start the program

```
systemctl enable --now hysteria
```

| project | |
| :--- | :--- |
| program | **/usr/local/bin/hysteria** |
| Placement | **/root/hysteria_config.yaml** |
| restart | `systemctl restart hysteria` |
| status | `systemctl status hysteria` |
| View log | `journalctl -u hysteria -o cat -e` |
| Real-time log | `journalctl -u hysteria -o cat -f` |

### uninstall

```
systemctl disable --now hysteria && rm -f /usr/local/bin/hysteria /root/hysteria_config.yaml /etc/systemd/system/hysteria.service
```

## 客户端

### by v2rayN provide HTTP SOCKS5 Deputy，by v2rayN provide road by rules

1. Download the Windows client program [hysteria-windows-amd64.exe](https://github.com/apernet/hysteria/actions/workflows/dev-build-hy2.yml)，Rename to hysteria.exe，Copy to v2rayN\bin\hysteria folder.
2. Download client configuration [config_client.yaml](https://github.com/chika0801/hysteria-install/blob/main/config_client.yaml)，modify chika.example.com Is the domain name contained in the certificate ，修改10.0.0.1为VPS的IP。

3. v2rayN：服务器 ——> 添加自定义配置服务器 ——> 浏览 ——> 选择客户端配置 ——> Core类型 hysteria ——> Socks端口 50000

![hysteria](https://github.com/chika0801/hysteria-install/assets/88967758/8044c172-7632-48f4-83ea-c711d688929d)

### by sing-box provide Tun mode（Transparent proxy），by sing-box Provide routing rules

1. sing-box：Reference [Windows How to use ](https://github.com/chika0801/sing-box-examples/blob/main/README.md)，Will client configuration](https://github.com/chika0801/sing-box-examples/blob/main/Tun/config_client_windows.json)Make the following changes。

原内容
```jsonc
        {
            "tag": "proxy",
            // Paste your client configuration，Need to keep "tag": "proxy",
        },
```

Replace with
```jsonc
        {
            "type": "socks",
            "tag": "proxy",
            "server": "127.0.0.1",
            "server_port": 50000
        },
```

2. v2rayN：server ——> Add a custom configuration server ——> browse ——> Select client configuration ——> Coretype hysteria ——> Socks port 0。
