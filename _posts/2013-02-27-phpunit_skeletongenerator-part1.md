---
layout: post
title: "PHPUnit_SkeletonGenerator part1"
description: ""
category: 
tags: PHPUnit PHP jenkins test
---
{% include JB/setup %}


## ざくっとまとめ
- ググると出てくるphpunit --skeleton, --skeleton-test は 少なくともPHPUnit version 3.7.14 には存在しない模様
    ↑ PHPUnit_SkeletonGeneratorで代用可能
    PHPUnit_SkeletonGenerator = クラスからテストケース作成よりテストケースからクラス作成が便利そう ->TDD!
    http://www.phpunit.de/manual/3.6/ja/skeleton-generator.html


## PHPUnit_SkeletonGenerator を使ってみる
    ./pear/pear -c .pearrc install phpunit/PHPUnit_SkeletonGenerator

## 例によってPATHを追加する
    vi ./libs/pear/phpunit-skelgen

## クラスからテストケース作成
    ./libs/pear/phpunit-skelgen --test App_Ro_News App/Ro/News.php

## テストケースからクラス作成
    ./libs/pear/phpunit-skelgen --class App_Ro_NewsTest
