# kcp-shadowsocks-docker

A docker image for [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) server with [KCPTUN](https://github.com/xtaci/kcptun) support

### Download from Docker Hub 

    docker pull imhang/kcp-shadowsocks-docker

### Usage

    docker run -p 443:443 -p 443:443/udp -p 9443:9443/udp --rm -it imhang/kcp-shadowsocks-docker

or running as a service

    docker run -d --restart=always -e "SS_PORT=443" -e "SS_PASSWORD=123456" -e "SS_METHOD=chacha20" -e "SS_TIMEOUT=600" -e "KCP_PORT=9443" -e "KCP_MODE=fast" -e "MTU=1400" -e "SNDWND=1024" -e "RCVWND=1024" -p 443:443 -p 443:443/udp -p 9443:9443/udp --name ssserver imhang/kcp-shadowsocks-docker

### KCP Parameters for client

    --crypt none --mode fast --mtu 1400 --sndwnd 128 --rcvwnd 512--parityshard 0 --nocomp

### Default configuration in environment variables

    SS_PORT     443

    SS_PASSWORD 123456

    SS_METHOD   chacha20

    SS_TIMEOUT  600

    KCP_PORT    9443

    KCP_KEY     123456

    KCP_CRYPT   none

    KCP_MODE    fast

    MTU         1400

    SNDWND      512

    RCVWND      128

    DATASHARD   10

    PARITYSHARD 0


compose 
```
curl -L https://github.com/docker/compose/releases/download/1.17.0-rc1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

```
```
ss:
  image: imhang/kcp-shadowsocks-docker:latest
  net: "host"
  restart: always
  environment:
    - SS_PORT=443
    - KCP_CRYPT=xor
    - KCP_PORT=9443
    - SS_METHOD=chacha20
    - KCP_MODE=fast
    - PARITYSHARD=0
    - DATASHARD=10
    - RCVWND=1024
    - SS_PASSWORD=123@root
    - KCP_KEY=123456
    - SS_TIMEOUT=600
    - SNDWND=1024
    - MTU=1400
```
sysctl -p

```
fs.file-max = 51200

net.core.rmem_max = 67108864
net.core.wmem_max = 67108864
net.core.netdev_max_backlog = 250000
net.core.somaxconn = 4096

net.ipv4.tcp_syncookies = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_tw_recycle = 0
net.ipv4.tcp_fin_timeout = 30
net.ipv4.tcp_keepalive_time = 1200
net.ipv4.ip_local_port_range = 10000 65000
net.ipv4.tcp_max_syn_backlog = 8192
net.ipv4.tcp_max_tw_buckets = 5000
net.ipv4.tcp_fastopen = 3
net.ipv4.tcp_mem = 25600 51200 102400
net.ipv4.tcp_rmem = 4096 87380 67108864
net.ipv4.tcp_wmem = 4096 65536 67108864
net.ipv4.tcp_mtu_probing = 1
net.ipv4.tcp_congestion_control = hybla
```
