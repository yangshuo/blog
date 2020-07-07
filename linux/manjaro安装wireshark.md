# Manjaro 安装 wireshark
* 1. 安装 wireshark

```bash
sudo pacman -S wireshark
```
* 2. 配置非root用户运行wireshark

```bash
sudo gpasswd -a $USER wireshark #把当前用户添加到wireshark组
```

