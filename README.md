由于~~怀疑被官方针对~~学习通频繁更新源码中的接口，为了确保所有功能可持续使用，此后将只发布最基础的签到代码，完整功能请登录学习通在线自动签到系统使用官方节点进行体验
# ChaoXing_node_signin
学习通在线自动签到系统签到节点接入脚本，用于自行部署可接入学习通在线自动签到系统的签到节点，该脚本需配合学习通在线自动签到系统使用，网址：https://cx.waadri.top/
## 使用方法
不喜欢折腾且使用Windows系统的用户可直接使用Releases中打包好的EXE文件运行，其它用户可在配置好python环境后参照下方步骤操作


### Python 运行
安装好python3环境后

安装依赖
```bash
pip install --user -r requirements.txt
```

使用以下命令运行
```bash
python other-signin.py
```

首次运行会自动在脚本目录下生成node_config.yaml配置文件，修改配置后再次运行脚本即可上线节点。默认配置文件如下所示
```yaml
# 邮件功能配置区
email:
  # 用来发送邮件的邮箱，未填写则不发送邮件
  address: ''
  # 用来发送邮件的邮箱密码
  password: ''
  # 是否使用tls加密连接，默认为true
  use_tls: true
  # 邮件服务器的host主机名
  host: ''
  # 邮件服务器端口
  port: 465
  # 发件人名称
  user: ''
# 节点名称密码配置区
node:
  # 节点名称，不能和已接入在线自动签到系统的其它自建节点名称重复
  name: ''
  # 节点密码，设置后用户需要在网站中输入正确的密码才能使用该节点，留空则为不设置密码，此时任何人均可使用该节点进行签到
  password: ''
# 是否启用debug模式，启用后日志输出更加详细，方便排查问题，建议使用时出现问题且命令行中未展示问题详细信息时再启用
debug: false
# 节点uuid，第一次使用时会随机生成，请勿更改
uuid: XXX
```

### Docker 运行

构建镜像
```bash
docker build -t waadri/chaoxing-auto-sign .
```

生成配置文件
```bash
docker run --name tmp --rm -v $(pwd):/host waadri/chaoxing-auto-sign sh -c "python3 other-signin-node.py && cp node_config.yaml /host/"
```

修改配置文件后，运行
```bash
docker run --name=cx -it -v $(pwd)/node_config.yaml:/app/node_config.yaml waadri/chaoxing-auto-sign
```

亦可使用群友构建的镜像 `ccr.ccs.tencentyun.com/misaka-public/waadri-sign-node:amd64` 或 `ccr.ccs.tencentyun.com/misaka-public/waadri-sign-node:arm64`

## 使用

运行上线后可在在线自动签到系统中点击“其它第三方自选节点”，会自动弹出自选节点列表，选择并输入正确的密码后即可使用所选节点进行签到监控
![image](https://github.com/WAADRI/ChaoXing_node_signin/assets/90495619/3f48708a-8e71-4147-8005-c4a266782014)
### 注意事项
- 仅供学习交流，不要用于非法用途
- 为防止部分接口被恶意利用，第三方节点不支持手动签到模式，不支持开启签到监控状态下自动更新用户签到信息（签到过程中仍可在网站上修改签到信息但不会自动同步至第三方节点服务端）。为防止官方更新导致功能失效，第三方节点不再支持反钓鱼签到模式和签到信息盗用模式，不再支持使用接口4进行签到监控，不再支持自动获取签到手势和签到码，这两种签到需使用官方节点或自行在学习通APP中签到，不再支持自动获取指定位置签到和指定位置二维码签到的指定位置信息，因此指定位置签到将使用用户在系统中所设置的位置信息尝试进行签到，指定位置二维码签到需在扫码小程序中自行选择位置并扫码，不再支持滑块验证码自动通过并签到，仅官方节点支持自动通过滑块验证码。除此以外第三方节点不支持节点离线后自动更换签到节点，且无7无签到自动停止签到监控的限制
- 以上限制为第三方签到节点的限制，此番举措为防止官方更新导致功能失效，若要体验最完整的功能还请使用官方节点进行签到监控
- 节点脚本不要搭建在云服务器厂商的IP环境下，否则可能会被学习通官方封禁IP地址导致无法进行签到监控，请在四大运营商或教育网的网络环境下搭建
- 如有侵权请联系我们删除，Email：WiFi86@qq.com
