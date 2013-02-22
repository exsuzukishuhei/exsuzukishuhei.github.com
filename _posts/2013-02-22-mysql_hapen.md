---
layout: post
title: "MySQL Tips"
description: ""
category: 
tags: []
---
{% include JB/setup %}

## 参照でテーブルがロックされてしまう
<pre>
特定のデータベースのみ参照系のSQLすら結果を返さない状態
</pre>

### 原因
- ChromeでAPIサーバを叩いていたのが原因

### 対応策
- Chromeを開き直したら改善された Ctrl + q

### 原因特定までの仮定
- DBに接続してプロセスチェック
<pre>
SHOW PROCESSLIST\G
</pre>
- SELECTだけなのに３０件超も待機SQLがあったので下記サイトを確認して原因ぽいプロセスを`kill PROCESS_ID`
  - http://dev.mysql.com/doc/refman/5.1/ja/show-processlist.html
- でも改善されず・・・
  - なんどか試したもののKILLプロセス自体も残ってしまう状態だった
- とりあえずSELECTしたAPIの実装を疑ってみる
  - 特に変なところなし - ブラウザを疑ってみる
  - ビンゴ！

