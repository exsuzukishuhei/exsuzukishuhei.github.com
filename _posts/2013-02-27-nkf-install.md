---
layout: post
title: "nkf インストールメモ"
description: ""
category: 
tags: server command nkf 
---
{% include JB/setup %}

## what nkf?
- 文字コード確認・変換コマンド

## インストール
    mkdir -p ~/local/{src,bin}
    cd ~/local/src
    wget -O nkf.tgz 'http://sourceforge.jp/frs/redir.php?m=keihanna&f=%2Fnkf%2F53171%2Fnkf-2.1.2.tar.gz'
    tar xzvf nkf.tgz
    cd nkf-2.1.2
    make
    cp nkf ~/local/bin

## 使用例
    # 文字コード確認
    nkf -g hoge.html
    # UTF-8に変換
    nkf -w --overwrite hoge.hml
