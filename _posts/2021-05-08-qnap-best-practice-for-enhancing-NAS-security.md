---
layout: post
title:  "QNAP best practice for enhancing NAS security"
subtitle: "QNAP のセキュリティ向上のためのベストプラクティスを実施してみた(画像付き)"
date:   2021-05-08 00:00:00
categories: [tips]
---


## 概要
<span style="font-size: 80%;">ランサムウェア(Qlocker とeCh0raix)に狙われがちなQNAP が、公式でセキュリティ向上のためのベストプラクティスを公開しています。本ブログでは、ベストプラクティスの手順を画像付きで、分かり易く記載しました。大切なデータを守るための、大切な作業のお役に立てていただければ嬉しいです。</span>

- [NAS セキュリティを向上するためのベストプラクティスは何ですか？](https://www.qnap.com/ja-jp/how-to/faq/article/nas-%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%92%E5%90%91%E4%B8%8A%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AE%E3%83%99%E3%82%B9%E3%83%88%E3%83%97%E3%83%A9%E3%82%AF%E3%83%86%E3%82%A3%E3%82%B9%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B)

<span style="font-size: 80%;">取り急ぎ、Qlocker の対策にフォーカスしたい！という方は、QNAP 公式が画像付きで手順を公開していますので、下記リンクをご確認下さい。折を見て、本手順を実施してくださいね。</span>

- [NASを Qlocker ランサムウェア攻撃から守る予防法＆対処法](https://note.com/qnap/n/n2e6f7106921e) 

***

## 実施環境
- モデル：TS-231
- 更新前のファームウェアバージョン：4.3.4.0597

***

## 実施時間
<span style="font-size: 80%;">本ブログ作成時の作業時間は**3時間程度**でした。(NAS の状態やネットワーク回線に依存しますので、参考程度にお考え下さい。)</span>

***

## 手順
<span style="font-size: 80%;">QNAP 公式ページの手順に従って記載していますが、一部手順の追加しております。なお、画像は上記「実施環境」にて実施した際の画面となります。モデルやファームウェアのバージョンによって、画像とは異なる画面が表示されることがありますので、ご注意ください。</span>

### 1. 不明なユーザーや疑わしいユーザーの削除
  1. 管理者としてQTS にログインします。
  2. コントロールパネル > 権限設定 > ユーザ に移動します。
  ![1-2-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_1-2-1.png)
  ![1-2-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_1-2-2.png)
  3. リスト上のすべてのユーザーを確認します。
  4. 不明なユーザーや疑わしいユーザーを選択します。
  ![1-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_1-4.png)
  5. 「削除」をクリックします。
  ![1-5]({{site.baseurl}}/images/qnap_bp/qnap_bp_1-5.png)
  6. 確認のメッセージが表示されるので、「OK」をクリックします。
  ![1-6]({{site.baseurl}}/images/qnap_bp/qnap_bp_1-6.png)


### 2. 不明なアプリケーションや疑わしいアプリケーションの削除
  1. 管理者としてQTS にログインします。
  2. App Center を開きます。
  ![2-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_2-2.png)
  3. インストールされているすべてのアプリケーションを確認します。
  ![2-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_2-3.png)
  4. 不明なアプリケーションや疑わしいアプリケーションを見つけます。
  5. アプリケーションアイコンの「下矢印」をクリックして「削除」を選択します。
  ![2-5]({{site.baseurl}}/images/qnap_bp/qnap_bp_2-5.png)
  6. 確認のメッセージが表示されるので、「OK」をクリックします。
  ![2-6]({{site.baseurl}}/images/qnap_bp/qnap_bp_2-6.png)


### 3. myQNAPcloud 設定の変更
  <span style="font-size: 80%;"> 3.4.以降はmyQNAPcloud サービスを利用していない場合は実施不要です。myQNAPcloud を開いた際に、以下の表示がある場合は、現在myQNAPcloud サービスを利用していないことを示します。</span>

  ![3]({{site.baseurl}}/images/qnap_bp/qnap_bp_3.png) 

  1. 管理者としてQTS にログインします。
  2. myQNAPcloud を開きます。
  ![3-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_3-2.png) 
  3. 「自動ルーター構成」に移動し、「UPnPポート転送の有効化」の選択を解除します。
  ![3-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_3-3.png) 
  4. 「サービスの公開」に進みます。
  5. 不要なサービスの選択をすべて解除します。
  6. 「適用」をクリックします。
  7. 「アクセス制御」に移動します。
  8. 「デバイスアクセス制御」を「プライベート」に設定します。
  9. 「適用」をクリックします。

### 4. システムポート番号の変更
#### NAS をインターネットに直接接続している場合（例：PPPoE、静的外部IPアドレス、DMZ モードのルーターなど）、本手順を実施してください。
#### NAS をルーターのポート転送機能で、インターネットに接続している場合は、ルーターのポート転送設定で、新しいポート番号を指定してください。22、443、80、8080、または8081は使用しないでください。

  1. 管理者としてQTS にログインします。
  2. コントロールパネル > システム > 全般設定 > システム管理 に移動します。
  ![4-2-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_4-2-1.png)
  ![4-2-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_4-2-2.png)
  ![4-2-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_4-2-3.png)
  3. 新しいシステムポート番号を指定し、「適用」をクリックします。(22、443、80、8080、または8081は使用しないでください。)
  ![4-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_4-3.png)
　

### 5. 最新版のMalware Remover のインストールと実行
  1. 管理者としてQTS にログインします。
  2. App Center を開きます。
  ![5-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-2.png)
  3. 検索アイコンをクリックし、検索ボックスに「Malware Remover」と入力してからENTER を押します。
  ![5-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-3.png)
  4. 検索結果にMalware Removerアプリケーションが表示されるので、「インストール」をクリックします。
  ![5-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-4.png) 
  5. 最新版のMalware Remover がインストールされます。
  ![5-5]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-5.png)
  6. インストール完了後、Malware Remover を開きます。
  ![5-6]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-6.png)
  7. 「スキャン開始」をクリックします。
  ![5-7-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-7-1.png)
  ![5-7-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-7-2.png)
  8. スキャン完了後「ログの表示」をクリックし、マルウェア検知に関連するログが出力されていないか確認する。
  ![5-8-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-8-1.png)
  ![5-8-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_5-8-2.png)


### 6. 管理者パスワードの変更
  1. 管理者としてQTS にログインします。
  2. QTS タスクバーのプロファイル写真をクリックします。
  ![6-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_6-2.png)
  3. 「オプション」ウィンドウ上部の「パスワードの変更」をクリックします。
  ![6-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_6-3.png)
  4. 古いパスワードと新しいパスワードを入力し、「適用」をクリックします。
  ![6-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_6-4.png)
  5. 自動的にログアウトするため、新しいパスワードでログインし直します。
  ![6-5]({{site.baseurl}}/images/qnap_bp/qnap_bp_6-5.png)

  - QNAP では、パスワードの強度を向上するためにも以下の条件に従うことをおすすめします。
    - 長さは最低でも8文字にする
    - 大文字と小文字の両方を含める
    - 最低1つの数字と1つの特殊文字を含める
    - ユーザー名や、すでに設定されているユーザー名と同じにしない
    - 3文字以上の繰り返しの文字は含めない


### 7. ユーザーパスワードの変更
  1. 管理者としてQTS にログインします。
  2. コントロールパネル > 権限設定 > ユーザ に移動します。
  ![7-2-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_7-2-1.png)
  ![7-2-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_7-2-2.png)
  3. パスワードを変更するユーザーの「鍵アイコン」をクリックします。
  ![7-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_7-3.png)
  4. 「パスワード変更」ウインドウが表示されるので、新しいパスワードを入力し、「適用」をクリックします。
  ![7-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_7-4.png)
  5. 上記の手順を繰り返し、その他ユーザーのパスワードを変更します。  

  - QNAP では、パスワードの強度を向上するためにも以下の条件に従うことをおすすめします。
    - 長さは最低でも8文字にする
    - 大文字と小文字の両方を含める
    - 最低1つの数字と1つの特殊文字を含める
    - ユーザー名や、すでに設定されているユーザー名と同じにしない
    - 3文字以上の繰り返しの文字は含めない


### 8. インストールされているQTSアプリケーションの更新
  1. 管理者としてQTS にログインします。
  2. App Center を開きます。
  ![8-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-2.png)
  3. 「マイアプリ」に進みます。
  ![8-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-3.png)
  4. 「更新のインストール」で、「すべて」をクリックします。
  ![8-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-4.png)
  5. 確認のメッセージが表示されので、「OK」をクリックします。
  ![8-5]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-5.png)
  6. インストールされているアプリケーションが最新バージョンに更新されます。
  ![8-6-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-6-1.png)
  ![8-6-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_8-6-2.png)


### 9. QTSの更新
  1. 管理者としてQTS にログインします。
  2. コントロールパネル > システム > ファームウェア更新に移動します。
  ![9-2-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-2-1.png)
  ![9-2-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-2-2.png)
  3. 「ライブ更新」タブの「更新の確認」ボタンをクリックします。
  ![9-3]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-3.png)
  4. 確認のメッセージが表示されので、「OK」をクリックします。（NAS が再起動可能な状態の場合は「この更新に必要な場合、自動的にシステムを再起動します」にチェックを入れます。）
  ![9-4]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-4.png)
  5. リリースノートが表示されるため、「続行」をクリックするとファームウェアの更新が始まります。
  ![9-5-1]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-5-1.png)
  ![9-5-2]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-5-2.png)
  6. 更新完了後、1～3を実施してファームウェアが最新であることを確認します。4.のメッセージが表示される場合は、ファームウェアが最新になるまでファームウェアの更新を行います。
  ![9-6]({{site.baseurl}}/images/qnap_bp/qnap_bp_9-6.png)

#### ファームウェアの更新後、「8.インストールされているQTSアプリケーションの更新」を実施し、アプリケーションを最新バージョンに更新します。

***

## 参考サイト 
- [NASを Qlocker ランサムウェア攻撃から守る予防法＆対処法](https://note.com/qnap/n/n2e6f7106921e)
- [NAS セキュリティを向上するためのベストプラクティスは何ですか？](https://www.qnap.com/ja-jp/how-to/faq/article/nas-%E3%82%BB%E3%82%AD%E3%83%A5%E3%83%AA%E3%83%86%E3%82%A3%E3%82%92%E5%90%91%E4%B8%8A%E3%81%99%E3%82%8B%E3%81%9F%E3%82%81%E3%81%AE%E3%83%99%E3%82%B9%E3%83%88%E3%83%97%E3%83%A9%E3%82%AF%E3%83%86%E3%82%A3%E3%82%B9%E3%81%AF%E4%BD%95%E3%81%A7%E3%81%99%E3%81%8B)
- [Qlockerによるランサムウェア攻撃に対するご案内：QNAP NASを安全にお使いいただくために](https://www.qnap.com/ja-jp/news/2021/qlocker-%E3%81%AB%E3%82%88%E3%82%8B%E3%83%A9%E3%83%B3%E3%82%B5%E3%83%A0%E3%82%A6%E3%82%A7%E3%82%A2%E6%94%BB%E6%92%83%E3%81%AB%E5%AF%BE%E3%81%99%E3%82%8B%E3%81%94%E6%A1%88%E5%86%85qnap-nas-%E3%82%92%E5%AE%89%E5%85%A8%E3%81%AB%E3%81%8A%E4%BD%BF%E3%81%84%E3%81%84%E3%81%9F%E3%81%A0%E3%81%8F%E3%81%9F%E3%82%81%E3%81%AB)

***

