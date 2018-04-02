---
layout: post
title: "Mac版 Navicat Premium 12.0.25 激活"
category :
- 杂项
tagline: "Navicat Premium"
tags : [工具使用,Navicat]
---

详细请参考： [Github-navicat-keygen](https://github.com/DoubleLabyrinth/navicat-keygen)

```
git clone https://github.com/DoubleLabyrinth/navicat-keygen.git
cd navicat-keygen
make release

cp /Applications/Navicat Premium.app/Contents/Resources/rpk /Applications/Navicat Premium.app/Contents/Resources/rpk.bak

openssl genrsa -out 2048key.pem 2048
openssl rsa -in 2048key.pem -pubout -out rpk

cp rpk /Applications/Navicat Premium.app/Contents/Resources/rpk

./navicat-keygen 2048key.pem # 记录 snKey，此处终端不关闭

# 使用 snKey 激活Navicat Premium并选择手动激活
# 复制弹出框中的请求码到 之前未关闭的终端，Enter，获取注册码
# 将注册码粘贴到Navicat Premium 激活对话框，点击激活即可

```
