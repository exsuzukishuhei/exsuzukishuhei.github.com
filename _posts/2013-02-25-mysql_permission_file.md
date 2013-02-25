---
layout: post
title: "MySQLでSELECT文からCSVファイル生成してSolrに流しこむ"
description: ""
category: 
tags: mysql csv solr
---
{% include JB/setup %}

## 概要
- Solrに大量のデータを入れなおしたかった
- テスト環境でPOSTして入れていくバッチ書いたものの速度が出ずorz...
- SolrのハンドラにCSVRequestHandlerなるものがあったのでそっちのほうが早そう!
- MySQLからSELECTしてCSVファイルに書きだすのどうやるんだっけ　←いまここ

## 作業ログ

### アカウントの権限チェック
- `FILE権限`が必要
- `GRANT ALL PRIVILEGES`ではFILE権限が付与されないので注意！

    GRANT FILE TABLES ON テーブル名.* TO アカウント名@'%'

### SELECT文からCSVファイル生成
    SELECT code, contents, created_at, updated_at FROM data INTO OUTFILE "/tmp/" FIELDS TERMINATED BY ','
    OPTIONALLY ENCLOSED BY '"';

### Solrに投げる前にCSVファイルを整形
- 1行目にフィールド名をいれておく

    code,contents,created_at, updated_at

### solrconfig.xmlにハンドラとディスパッチャーの設定を追加
    <requestDispatcher handleSelect="true">
        <requestParsers enableRemoteStreaming="true" multipartUploadLimitInKB="60000" />
    </requestDispatcher>
    <requestHandler name="/update/csv" class="solr.CSVRequestHandler" startup="lazy"/>

### Solrに流しこむ
    curl 'http://localhost:8080/solr/update/csv?stream.file=/tmp/data.cvs&stream.contentType=text/plain;charset=utf-8'
    curl 'http://localhost:8080/solr/update/csv?commit=true'
