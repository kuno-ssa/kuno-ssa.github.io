---
layout: post
title:  "subdomain enum services"
subtitle: "Passive＆Freeでインターネットに公開されているホストを列挙!"
date:   2020-03-27 00:00:01
categories: [osint]
---

<font color="red">※本投稿に記載の内容は犯罪行為を助長するものではありません。必ず自身の管理下にあるネットワークやサーバーに対してのみ実施してください。</font>

あるドメインについて、攻撃者目線でセキュリティチェックを行うためには、まず、対象ドメインにどんなホストがいくつ存在するのかを調べる必要があります。

対象ドメインのホスト、つまりサブドメインを列挙する方法はいくつかありますが、本ポストでは、公開情報を元にサブドメインを列挙するサービスにフォーカスし、数多あるサービスの中で、フリーでそこそこ使える以下の4つのサービスを紹介します。

1.virustotal(★★)  
2.hackertarget(★)  
3.Security Trails(★★)  
4.RiskIQ(★★★)  


※カッコ内の★は個人的オススメ度です。★～★★★の範囲で評価しており、★が増える毎にオススメ度が増します。

***

# 1. virustotal
### 1. サービスについて
- 概要：不審なファイルやURLを分析してマルウェアの種類を検出するサービス
- URL：[https://www.virustotal.com](https://www.virustotal.com)

### 2. Webインターフェース
- 費用：無償。ユーザ登録不要。
- 制限：特記事項なし

### 3. 無償API
- リクエスト数の制限：  
    4 req/min  
    1000 req/day  
    30000 req/month
- その他制限：商用利用不可

### 4. 有償API
- 最安プラン：要問合せ
- 費用：要問合せ
- リクエスト数の制限：プランによる

### 5. APIのリクエスト方法
- #### API REQUEST
    - URL: https://www.virustotal.com/api/v3/domains/<#domain>>/subdomains?limit=40  
        ※limitは結果の表示数。デフォルトは10で最大40。
    - Method: GET
    - HEADER: x-apikey:<#api-key>

- #### API RESPONSE
{% highlight json %}
# Example Response
{
    "data": [
        {
            "attributes": {…(中略)
           },
            "id": "www.joshisec.com",
            "links": {
                "self": "https://www.virustotal.com/api/v3/domains/www.joshisec.com"
            },
            "type": "domain"
        }
    ],
    "links": {
        "self": "https://www.virustotal.com/api/v3/domains/joshisec.com/subdomains?limit=40"
    }
}
{% endhighlight %}

- #### API Document
    - [https://developers.virustotal.com/v3.0/reference#domains-relationships](https://developers.virustotal.com/v3.0/reference#domains-relationships)

### 6.コメント  
- ◎ サブドメインに対応するIPアドレスを取得できるため、次ステップの調査工数が減る。
- ◎ WHOIS情報や、各セキュリティ製品のBlackListに同ドメインが登録されているかを確認できる。
- △ 1分に4リクエストしかできないため、大量のドメインを調査する場合には、それなりの時間を要する。
- △ 列挙可能なサブドメイン数が最大100件であるため、大量のサブドメインを保持しているドメインの調査には向いていない。(apple.comなど)

***

# 2. hackertarget
### 1. サービスについて
- 概要：オンライン脆弱性スキャナサービス(IP Tools APIという機能を利用)
- URL：[https://hackertarget.com/](https://hackertarget.com/)

### 2. Webインターフェース
- URL：[https://hackertarget.com/find-dns-host-records/](https://hackertarget.com/find-dns-host-records/)
- 費用：無償。ユーザ登録不要。
- 制限：特記事項なし

### 3. 無償API
- リクエスト数の制限：100 req/day
- その他制限：  
    商用利用不可  
    1リクエストの最大結果表示数が500まで

### 4. 有償API
- 最安プラン：Starter
- 費用：US$10/month
- リクエスト数の制限：500 req/day(IP Tools API機能全体でカウント)

### 5. APIのリクエスト方法(無償)
- #### API REQUEST
    - URL: https://api.hackertarget.com/hostsearch/?q=<#domain>
    - Method: GET

- #### API RESPONSE
{% highlight json %}
# Example Response
joshisec.com,157.7.107.46
www.joshisec.com,157.7.107.46
{% endhighlight %}

- #### API Document
    - [https://hackertarget.com/find-dns-host-records/](https://hackertarget.com/find-dns-host-records/)

### 6.コメント  
- ◎ サブドメインに対応するIPアドレスを取得できるため、次ステップの調査工数が減る。
- ◎ 結果がシンプルなので、スクリプト作成が容易。
- ◎ 列挙するサブドメイン数は、他の3サービスと比べてやや少ない印象。
- △ 列挙可能なサブドメインの数が最大500件であるため、大量のサブドメインを保持しているドメインの調査には向いていない。(apple.comなど)

***

# 3. Security Trails
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
    - URL: https://api.securitytrails.com/v1/domain/<#domain>/subdomains
    - Method: GET
    - HEADER: apikey:<#api-key>

- #### API RESPONSE
{% highlight json %}
# Example Response
{
  "subdomains": [
    "www"
  ],
  "endpoint": "/v1/domain/joshisec.com/subdomains"
}
{% endhighlight %}

- #### API Document
    - [https://docs.securitytrails.com/reference#domain-subdomains](https://docs.securitytrails.com/reference#domain-subdomains)

### 6.コメント  
- ◎ 列挙するサブドメインの数は、わりと安定している印象。
- △ 列挙可能なサブドメインの数が最大2000件であるため、大量のサブドメインを保持しているドメインの調査には向いていない。(しかしながら、Security Trailsいわく、2000を超えるサブドメインを保持しているドメインはそうないとのことなので、ごく一部のドメインを除けば、充分なのかもしれない) 
- △ 1か月に利用可能なリクエスト数が少ないため、大事に使う必要がある。

***

# 4. RiskIQ
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
    - URL: https://api.passivetotal.org/v2/enrichment/subdomains?query=<#domain>
    - Method: GET
    - HEADER: Authorization:Basic <#auth-key>

- #### API RESPONSE
{% highlight json %}
# Example Response
{
    "primaryDomain": "joshisec.com",
    "success": true,
    "queryValue": "joshisec.com",
    "subdomains": [
        "www",
        "joshisec.com"
    ]
}
{% endhighlight %}

- #### API Document
    - [https://api.passivetotal.org/index.html#api-Enrichment-GetV2EnrichmentSubdomains](https://api.passivetotal.org/index.html#api-Enrichment-GetV2EnrichmentSubdomains)

### 6.コメント  
- ◎ 列挙可能なサブドメインの数に制限がないとに思われる。大量のサブドメインを保持するドメインの調査に向いている。
- ◎ 検出するサブドメイン数が非常に多い。ただし、まれに第三者アクティブスキャンによるスカ情報も記録しているため、見極めが重要。
- △ 1日に利用可能なリクエスト数が少ないため、大事に使う必要がある。

***


