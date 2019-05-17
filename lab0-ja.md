# Lab 0 - Docker をインストールする

© Copyright IBM Corporation 2017

IBM、IBM ロゴ、ibm.com は、世界の多くの国で登録された International Business Machines Corp. の商標です。他の製品名およびサービス名等は、それぞれ IBM または各社の商標である場合があります。現時点での IBM の商標リストについては、Web 上で公開されている「Copyright and trademark information」(www.ibm.com/legal/copytrade.shtml) をご覧ください。

本文書は初版日のままの状態であり、将来 IBM が変更する可能性があります。

ここに記載する情報は、情報提供のみを目的に現状のままで提供するものとし、明示的もしくは黙示的かを問わず、一切の保証責任は適用されないものとします。IBM はここに記載した情報の使用に起因するいかなる損害についても責任を負いません。本文書は、IBM (または IBM のサプライヤーまたはライセンサー) にいかなる保証責任を負わせるものではなく、また、IBM ソフトウェアの使用に際し適用される、プログラムのご使用条件の内容を変更するものでもありません。本文書に記載の製品、プログラム、またはサービスが日本においては提供されていない場合があります。本文書は現行の IBM の製品プランまたは戦略に基づくものです。この製品プランまたは戦略は予告なく変更されることがあります。本文書で参照する製品のリリース日や機能は、市場機会やその他の要素に基づき、IBM が独自の裁量により随時変更する場合があり、今後の製品や機能の可用性を確約するものではありません。

# 概要

このラボでは、コースの残りのラボ全体を通して使用する Docker をインストールします。 

## 前提条件

Docker IDが下記で必要です
- Dockerのインストール
- Docker をインストールせずに[Play with Docker](http://play-with-docker.com) の使用する場合
- Lab2( [Docker Hub](https://hub.docker.com/) を使用するため)
- Lab3( [Play with Docker](http://play-with-docker.com)を使用するため)

お持ちでない場合は https://hub.docker.com/ より [[Sign up for Docker Hub]](https://hub.docker.com/signup)をクリックして作成してください。登録にはメールアドレスが必要です。
Webでの登録作業完了後、登録したメールアドレスにメールが送付されますので、そのメールにあるメールアドレスを確認するリンクをクリックしてアカウント登録が完了します。

> Dockerのインストール要件:
> https://docs.docker.com/install/#supported-platforms より利用するOSをクリックし、確認できます(英語)。インストールできない場合は 下記オプションの[Play with Docker](http://play-with-docker.com) をご使用ください。

# ステップ 1: Docker をインストールする

1. https://hub.docker.com/ の’Sign in’をクリックし、サインインします(Docker IDとパスワードが必要です)。

2. [Get Started with Docker Desktop]のボタンをクリックして、モジュールをダウンロードし、インストールします。

尚、Docker には 2 つのインストール・バージョンが用意されています。「有償」バージョンは Docker Enterprise Edition (Docker EE)、「無償」バージョンは Docker Community Edition (Docker CE) です。このコースでは、Docker Community Edition で提供される無償の機能のみを使用します。

# **オプション:** Play with Docker を使用する
Docker をインストールしたくない場合は、代わりの方法として [Play with Docker](http://play-with-docker.com) を使用できます。Play with Docker は、Docker がインストールされているターミナルを、ブラウザーから直接実行できる Web サイトです。このコースのすべてのラボは Play with Docker 上で実行できますが、このコースを完了した後も Docker の詳細を探れるよう、ローカル・ホストに Docker をインストールすることをお勧めします。Play with Docker を使用する場合は、ブラウザー内で http://play-with-docker.com にアクセスします。

http://play-with-docker.com にアクセス後、Docker Hubにサインインしていない場合は`Login`ボタンが表示されます。`Login`ボタンをクリックしてDocker IDでサインインし、`Start`ボタンでアクセスできることを確認しておいてください（既にサインイン済みの場合は最初から`Start`ボタンが表示されます）。

# まとめ

Docker をインストールするか、http://play-with-docker.com の使い方を把握した後は、このコースの残りのラボに取り組んでください。

