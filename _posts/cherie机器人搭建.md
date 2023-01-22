







首先配置gocp，运行后输入02生成配置文件（不能输入2）

登录可以选择qrcode，不需要图形化实现。

（就是不输入qq号密码）

修改gocp的配置文件，把0.0.0.0改为127.0.0.1



win部署：

已知：ws端口不能保持8080，我这改成了4090即可（改大了也不行）



linux部署：

已知：账号密码直接登录登不上（即便配置文件直接复制粘贴）

使用QRcode也提示网络环境复杂

解决：用tinyproxy搭了个代理跑，问题解决



文件无法运行，直接运行文件提示命令不存在（不该出现的问题）

改成.sh运行，授予chmod +X

不需要加bash xxx.sh

直接./xxx.sh



appimage在docker里需要使用fuse而无法运行

解压后使用

```
./chieri_client.sh
```

然后进入子文件夹中找到执行的文件执行



docker commit xxxx newimage时提示invide，我怀疑是bug，使用

docker commit containerID newimage即可