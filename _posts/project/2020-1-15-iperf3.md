---
layout: post
title: Surge iOS最新测试版新增了iperf3客户端模式
category: project
description: 随笔小记
---
>What to Test:
>- A new feature has been added: iperf3 client mode.
>You may use it to benchmark the bandwidth. Different from the standalone iperf app, you may force the test to use a specified proxy.
>A quick guide:
>1. Install iperf3 on the proxy server.
>2. Run "iperf3 -s" within a screen or tmux session.
>3. Start iperf test with Surge. Leave the hostname field empty. 127.0.0.1 will be used and indicates the proxy server itself.”

* Ubuntu服务器安装开启：
   1. 安装iperf3命令：`sudo apt-get install iperf3`
   2. 开启iperf3命令：`iperf3 -s`