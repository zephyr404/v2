# v2fly

官网：https://www.v2fly.org/

## 安装和更新 V2Ray

参考：https://github.com/v2fly/fhs-install-v2ray/blob/master/README.zh-Hans-CN.md

```bash
// 安装可执行文件和 .dat 数据文件
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)
```

## 安装最新发行的 geoip.dat 和 geosite.dat

```bash
// 只更新 .dat 数据文件
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-dat-release.sh)
```

## 移除 V2Ray

```bash
bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove
```

## 操作

```bash
# 自启动
systemctl enable v2ray

# 启动
systemctl start v2ray

# 关闭
systemctl stop v2ray

# 重启
systemctl restart v2ray

# 查看状态
systemctl status v2ray
```

## 配置

### 服务端

```json
// 基础配置  确保 id 和端口与客户端一致才可以正常连接
{
    "inbounds": [
        {
            "port": 10086, // 服务器监听端口
            "protocol": "vmess",
            "settings": {
                "clients": [
                    {
                        "id": "b831381d-6324-4d53-ad4f-8cda48b30811"
                    }
                ]
            }
        }
    ],
    "outbounds": [
        {
            "protocol": "freedom"
        }
    ]
}
```

### 客户端

手动配置太低级，我们用订阅。

## 订阅

参考：https://github.com/2dust/v2rayN/wiki/

### 分享链接格式说明(ver 2)

```json
// 分享的链接（二维码）格式：vmess://(Base64编码的json格式服务器数据
{
	"v": "2",
	"ps": " 备注或别名  ",
	"add": "111.111.111.111",
	"port": "32000",
	"id": "1386f85e-657b-4d6e-9d56-78badb75e1fd",
	"aid": "100",           // alertId，默认0的时候客户端可以不填，否则客户端与服务端需保持一致
	"scy": "zero",          // 可以为none
	"net": "tcp",
	"type": "none",
	"host": "www.bbb.com",  // 可以为none
	"path": "/",            // 可以为不要该字段
	"tls": "tls",           // 可以为不要该字段
	"sni": "www.ccc.com"    // 可以为不要该字段
}
```

### 分享链接格式说明

- 订阅地址(url):普通可以正常访问的url(http|https)
- 此url返回的内容:[分享链接格式说明(ver 2)]的集合,以换行分割(可参考v2rayN批量导出分享链接),最后base64编码
- 订阅支持vmess/ss/socks

vmess://base64(Configuration)
ss://base64(Configuration)
socks://base64(Configuration)

上面数据再base64输出

### 例子

```json
{
  "v": "2",
  "ps": "订阅1",
  "add": "localhost",
  "port": "8080",
  "id": "67ed525c-bd8c-3caa-a247-d353783cd2b6",
  "aid": "0",
  "net": "ws",
  "type": "none",
  "host": "aaa.com",
  "path": "/v2ray",
  "tls": "bbb.com"
}
```

base64得到

```
ew0KICAidiI6ICIyIiwNCiAgInBzIjogIuiuoumYhTEiLA0KICAiYWRkIjogImxvY2FsaG9zdCIsDQogICJwb3J0IjogIjgwODAiLA0KICAiaWQiOiAiNjdlZDUyNWMtYmQ4Yy0zY2FhLWEyNDctZDM1Mzc4M2NkMmI2IiwNCiAgImFpZCI6ICIwIiwNCiAgIm5ldCI6ICJ3cyIsDQogICJ0eXBlIjogIm5vbmUiLA0KICAiaG9zdCI6ICJhYWEuY29tIiwNCiAgInBhdGgiOiAiL3YycmF5IiwNCiAgInRscyI6ICJiYmIuY29tIg0KfQ==
```

字符串前加上协议

```
// 单条订阅
vmess://ew0KICAidiI6ICIyIiwNCiAgInBzIjogIuiuoumYhTEiLA0KICAiYWRkIjogImxvY2FsaG9zdCIsDQogICJwb3J0IjogIjgwODAiLA0KICAiaWQiOiAiNjdlZDUyNWMtYmQ4Yy0zY2FhLWEyNDctZDM1Mzc4M2NkMmI2IiwNCiAgImFpZCI6ICIwIiwNCiAgIm5ldCI6ICJ3cyIsDQogICJ0eXBlIjogIm5vbmUiLA0KICAiaG9zdCI6ICJhYWEuY29tIiwNCiAgInBhdGgiOiAiL3YycmF5IiwNCiAgInRscyI6ICJiYmIuY29tIg0KfQ==

// 多条订阅换行分隔
vmess://ew0KICAi==
vmess://nRscyI6I==
vless://ImFpZCI6==
...
```

再次base64得到订阅内容

```
dm1lc3M6Ly9ldzBLSUNBaWRpSTZJQ0l5SWl3TkNpQWdJbkJ6SWpvZ0l1aXVvdW1ZaFRFaUxBMEtJQ0FpWVdSa0lqb2dJbXh2WTJGc2FHOXpkQ0lzRFFvZ0lDSndiM0owSWpvZ0lqZ3dPREFpTEEwS0lDQWlhV1FpT2lBaU5qZGxaRFV5TldNdFltUTRZeTB6WTJGaExXRXlORGN0WkRNMU16YzRNMk5rTW1JMklpd05DaUFnSW1GcFpDSTZJQ0l3SWl3TkNpQWdJbTVsZENJNklDSjNjeUlzRFFvZ0lDSjBlWEJsSWpvZ0ltNXZibVVpTEEwS0lDQWlhRzl6ZENJNklDSmhZV0V1WTI5dElpd05DaUFnSW5CaGRHZ2lPaUFpTDNZeWNtRjVJaXdOQ2lBZ0luUnNjeUk2SUNKaVltSXVZMjl0SWcwS2ZRPT0=
```

保证订阅域名可以解析到文本

### 备注

配置log后启动失败可能是由于权限不足导致的。

```bash
chmod -R 777 ./logs
chmod -R 777 ./logs/access.log
chmod -R 777 ./logs/error.log
```
