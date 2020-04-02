---
layout: post
title:  "domain record lookup services"
subtitle: "Passive＆Freeでドメインに紐づくIPアドレスの履歴を追う!"
date:   2020-04-02 00:00:00
categories: [osint]
---

<font color="red">※本投稿に記載の内容は犯罪行為を助長するものではありません。必ず自身の管理下にあるネットワークやサーバーに対してのみ実施してください。</font>

サブドメインの列挙が完了したら、次はそのサブドメインに紐づくIPアドレスを見つけます。なぜIPアドレスが必要かというと、次ステップで使用するサービスの検索がIPアドレスベースのためです。

さて、ドメインからIPアドレスに変換を行うだけであれば、nslookupをたたけば充分なのですが、このシリーズではあくまで<font color="red">パッシブ</font>にこだわります。また、該当DNSレコードが<font color="red">いつの時点で有効であったものか</font>を知る必要があるため、その情報が分かる、以下3サービスを紹介します。なお、紹介するサービスのうち1～2は、前ステップで既に紹介済みのため、サービスの内容など重複する箇所もありますが、ご容赦ください。
 
1.Security Trails(★★)  
2.RiskIQ(★★)  
3.AlienVault OTX(★★)

※カッコ内の★は個人的オススメ度です。★～★★★の範囲で評価しており、★が増える毎にオススメ度が増します。

***

# 1. Security Trails
### 1. サービスについて
- 概要：サイバーセキュリティインテリジェンスプラットフォーム
- URL：[https://securitytrails.com/](https://securitytrails.com/)

### 2. Webインターフェース
- 費用：無償。ユーザ登録不要。
- 制限：検索結果がDLできない等

### 3. 無償API
- リクエスト数の制限：  
    50 req/month  
    2 req/sec  
- その他制限：商用利用不可

### 4. 有償API
- 最安プラン：Prototyper
- 費用：US$50/month
- リクエスト数の制限：1500 req/month

### 5. APIのリクエスト方法
- #### API REQUEST
    - URL: https://api.securitytrails.com/v1/history/<#domain>/dns/a
    - Method: GET
    - HEADER: apikey:<#api-key>

- #### API SAMPLE
{% highlight json %}
# Sample Request
https://api.securitytrails.com/v1/history/www.joshisec.com/dns/a

# Sample Response
{
  "type": "a/ipv4",
  "records": [
    {
      "values": [
        {
          "ip_count": 777579,
          "ip": "185.199.111.153"
        },
        …(中略)…
        {
          "ip_count": 799833,
          "ip": "185.199.108.153"
        }
      ],
      "type": "a",
      "organizations": [
        "Fastly"
      ],
      "last_seen": "2020-04-02",
      "first_seen": "2020-03-30"
    },
    {
      "values": [
        {
          "ip_count": 3646,
          "ip": "157.7.107.46"
        }
      ],
      "type": "a",
      "organizations": [
        "GMO Internet,Inc"
      ],
      "last_seen": "2020-03-30",
      "first_seen": "2017-05-26"
    }
  ],
  "pages": 1,
  "endpoint": "/v1/history/www.joshisec.com/dns/a"
}
{% endhighlight %}

- #### API Document
    - [https://docs.securitytrails.com/reference#history-dns](https://docs.securitytrails.com/reference#history-dns)

### 6.コメント  
- ◎ DNSレコードの情報が新しい。
- ◎ 各レコードが検出された期間を確認でき、また、過去に登録されていたDNSレコードも確認できるため、次ステップで時間の整合性がとれる。
- △ 1か月に利用可能なリクエスト数が少ないため、大事に使う必要がある。

***

# 2. RiskIQ
### 1. サービスについて
- 概要：脅威分析プラットフォーム。パッシブDNSツールとしてあまりにも有名。
- URL：[https://community.riskiq.com/home](https://community.riskiq.com/home)

### 2. Webインターフェース
- 費用：無償だが、ユーザ登録が必要。
- 制限：15 req/day

### 3. 無償API
- リクエスト数の制限：15 req/day
- その他制限：特記事項なし(商用利用の可不可は私が確認した限りでは記載なし)

### 4. 有償API
- 最安プラン：要問合せ
- 費用：要問合せ
- リクエスト数の制限：要問合せ

### 5. APIのリクエスト方法
- #### API REQUEST
    - URL: https://api.passivetotal.org/v2/dns/passive?query=<#domain>
    - Method: GET
    - HEADER: Authorization:Basic <#auth-key>

- #### API SAMPLE
{% highlight json %}
# Sample Request
https://api.passivetotal.org/v2/dns/passive?query=www.joshisec.com

# Sample Response
{
    "pager": null,
    "queryValue": "www.joshisec.com",
    "queryType": "domain",
    "firstSeen": "2017-11-26 15:55:03",
    "lastSeen": "2020-04-02 03:06:05",
    "totalRecords": 6,
    "results": [
        {
            "firstSeen": "2017-11-26 15:55:03",
            "lastSeen": "2020-02-10 23:09:26",
            "source": [
                "riskiq"
            ],
            "value": "www.joshisec.com",
            "collected": "2020-04-02 10:06:04",
            "recordType": "A",
            "resolve": "157.7.107.46",
            "resolveType": "ip",
            "recordHash": "73494794b8eb7b76b084cbe0a008f41cbcb559d5bbc2a9c7e060ecf2f9c4ef98"
        },
        {
            "firstSeen": "2020-04-02 03:06:05",
            "lastSeen": "2020-04-02 03:06:05",
            "source": [
                "pingly"
            ],
            "value": "www.joshisec.com",
            "collected": "2020-04-02 10:06:05",
            "recordType": "A",
            "resolve": "185.199.109.153",
            "resolveType": "ip",
            "recordHash": "af0b7e952beb4cff03bb274226649898a81924b985f7ef12d0ee74bef75986c7"
        },
        …(中略)…
        {
            "firstSeen": "2020-04-02 03:06:05",
            "lastSeen": "2020-04-02 03:06:05",
            "source": [
                "pingly"
            ],
            "value": "www.joshisec.com",
            "collected": "2020-04-02 10:06:05",
            "recordType": "A",
            "resolve": "185.199.110.153",
            "resolveType": "ip",
            "recordHash": "197943b048358b38fcd4af6979b49cc5372fbeeec5cd8e8fe142d330045b1d7d"
        }
    ]
}
{% endhighlight %}

- #### API Document
    - [https://api.passivetotal.org/index.html#api-Passive_DNS-GetV2DnsPassive](https://api.passivetotal.org/index.html#api-Passive_DNS-GetV2DnsPassive)

### 6.コメント  
- ◎ DNSレコードの情報が新しい。
- ◎ 各レコードが検出された期間を確認でき、また、過去に登録されていたDNSレコードも確認できるため、次ステップで時間の整合性がとれる。
- △ 1日に利用可能なリクエスト数が少ないため、大事に使う必要がある。

***

# 3. AlienVault OTX
### 1. サービスについて
- 概要：世界初の本当にオープンな脅威インテリジェンスコミュニティ。完全無償。
- URL：[https://otx.alienvault.com](https://otx.alienvault.com)

### 2. Webインターフェース
- 費用：無償。ユーザ登録不要。

### 3. 無償API
- リクエスト数の制限※1：  
    ユーザ登録なしで使用する場合　1000 req/hour  
    ユーザ登録して使用する場合　10000 req/hour
- その他制限：特記事項なし

### 4. 有償API
- 完全無償のため存在せず。

### 5. APIのリクエスト方法
- #### API REQUEST
    - URL: https://otx.alienvault.com/api/v1/indicators/domain/<#domain>/passive_dns
    - Method: GET
    - HEADER: X-OTX-API-KEY:<#api-key>

- #### API SAMPLE
{% highlight json %}
# Sample Request
https://otx.alienvault.com/api/v1/indicators/domain/www.joshisec.com/passive_dns

# Sample Response
{
    "passive_dns": [
        {
            "last": "2020-01-30T08:20:40+00:00",
            "indicator_link": "/indicator/hostname/www.joshisec.com",
            "hostname": "www.joshisec.com",
            "record_type": "A",
            "address": "157.7.107.46",
            "flag_url": "assets/images/jp.png",
            "flag_title": "Japan",
            "asset_type": "hostname",
            "first": "2019-05-14T19:41:51+00:00"
        }
    ],
    "count": 1
}
{% endhighlight %}

- #### API Document
    - [https://otx.alienvault.com/api](https://otx.alienvault.com/api)

### 6.コメント  
- ◎ APIのリクエスト制限が大らかなため、気軽に使える。
- ◎ 各レコードが検出された期間を確認でき、また、過去に登録されていたDNSレコードも確認できるため、次ステップで時間の整合性がとれる。
- △ DNSレコードの情報がやや古い。上記の例では、検索時点から2ヶ月前の情報が最新となっている。


※1 [The Upgraded AlienVault OTX API & Ways to Score Swag!](https://cybersecurity.att.com/blogs/security-essentials/the-upgraded-alienvault-otx-api-ways-to-score-swag)

***

