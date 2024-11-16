# 系统代理

## mac快速设置系统代理

```shell
setproxy() {
    local ip=${1:-127.0.0.1}  # 默认 IP
    local port=${2:-8899}     # 默认端口

    # 设置系统代理
    networksetup -setwebproxy "Wi-Fi" "$ip" "$port"
    networksetup -setsecurewebproxy "Wi-Fi" "$ip" "$port"
    networksetup -setwebproxystate "Wi-Fi" on
    networksetup -setsecurewebproxystate "Wi-Fi" on

    # 设置环境变量以便终端应用使用代理
    export http_proxy="http://$ip:$port"
    export https_proxy="http://$ip:$port"
    export all_proxy="socks://$ip:$port"  # 如果需要 SOCKS 代理

    echo "Proxy set to $ip:$port"
}

unsetproxy() {
    networksetup -setwebproxystate "Wi-Fi" off
    networksetup -setsecurewebproxystate "Wi-Fi" off

    # 清除环境变量
    unset http_proxy
    unset https_proxy
    unset all_proxy

    echo "Proxy disabled"
}
```