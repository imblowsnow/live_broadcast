# live_broadcast
利用 streamlink + ffmpeg 转播流到任意平台

# 安装教程
## 安装streamlink
https://github.com/streamlink/streamlink

> 官网安装教程：https://streamlink.github.io/install.html
### windows

```bash
pip install --user -U streamlink 
```

### unbuntu
```
apt install streamlink
```

## 安装ffmpeg
```shell
wget https://johnvansickle.com/ffmpeg/releases/ffmpeg-release-amd64-static.tar.xz
tar -xJf ffmpeg-release-amd64-static.tar.xz
cp -r ffmpeg-6.0-amd64-static /usr/local/ffmpeg
```

> 在~/.bashrc文件添加一行
```shell
export PATH=$PATH:/usr/local/ffmpeg
```
> 然后运行使其生效

```shell
source .bashrc
```


# 转播教程
```bash
nohup streamlink -O [采集地址] best | ffmpeg -re -i pipe:0 -c:v copy -c:a aac -f flv [推流地址] > /home/liveBroadcast.log 2>&1 &
```
- [采集地址]：youtube 等，具体支持站点查看：https://streamlink.github.io/plugins.html
- [推流地址]：推送的地址，B站等

# 服务守护
替换下面的 ExecStart 为上面自己修改的命令

新增文件
/usr/lib/systemd/system/liveBroadcast.service

```service
[Unit]
Description=rtmp push
After=network.target

[Service]
Type=forking
ExecStart=streamlink -O [采集地址] best | ffmpeg -re -i pipe:0 -c:v copy -c:a aac -f flv [推流地址] > /home/liveBroadcast.log 2>&1 &
Restart=always
[Install]

WantedBy=multi-user.target
```
