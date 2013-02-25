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

<pre><code>
GRANT FILE TABLES ON テーブル名.* TO アカウント名@'%'
</code></pre>

### SELECT文からCSVファイル生成
<pre><code>
SELECT code, contents, created_at, updated_at FROM data INTO OUTFILE "/tmp/" FIELDS TERMINATED BY ','
OPTIONALLY ENCLOSED BY '"';
</code></pre>

### Solrに投げる前にCSVファイルを整形
- 1行目にフィールド名をいれておく

<pre>
code,contents,created_at, updated_at
</pre>

### solrconfig.xmlにハンドラとディスパッチャーの設定を追加
<pre><code>
 <requestDispatcher handleSelect="true">
   <requestParsers enableRemoteStreaming="true" multipartUploadLimitInKB="60000" />
 </requestDispatcher>
 <requestHandler name="/update/csv" class="solr.CSVRequestHandler" startup="lazy"/>
</code></pre>

### Solrに流しこむ
<pre><code>
curl 'http://localhost:8080/solr/update/csv?stream.file=/tmp/data.cvs&stream.contentType=text/plain;charset=utf-8'
curl 'http://localhost:8080/solr/update/csv?commit=true'
</code></pre>
