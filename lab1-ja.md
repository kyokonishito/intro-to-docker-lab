# ラボ 1 - 初めてのコンテナーを実行する

© Copyright IBM Corporation 2017

IBM、IBM ロゴ、ibm.com は、世界の多くの国で登録された International Business Machines Corp. の商標です。他の製品名およびサービス名等は、それぞれ IBM または各社の商標である場合があります。現時点での IBM の商標リストについては、Web 上で公開されている「Copyright and trademark information」(www.ibm.com/legal/copytrade.shtml) をご覧ください。

本文書は初版日のままの状態であり、将来 IBM が変更する可能性があります。

ここに記載する情報は、情報提供のみを目的に現状のままで提供するものとし、明示的もしくは黙示的かを問わず、一切の保証責任は適用されないものとします。IBM はここに記載した情報の使用に起因するいかなる損害についても責任を負いません。本文書は、IBM (または IBM のサプライヤーまたはライセンサー) にいかなる保証責任を負わせるものではなく、また、IBM ソフトウェアの使用に際し適用される、プログラムのご使用条件の内容を変更するものでもありません。本文書に記載の製品、プログラム、またはサービスが日本においては提供されていない場合があります。本文書は現行の IBM の製品プランまたは戦略に基づくものです。この製品プランまたは戦略は予告なく変更されることがあります。本文書で参照する製品のリリース日や機能は、市場機会やその他の要素に基づき、IBM が独自の裁量により随時変更する場合があり、今後の製品や機能の可用性を確約するものではありません。

# 概要

このラボでは、皆さんにとって初めてとなる Docker コンテナーを実行します。 

コンテナーとは、隔離された状態で実行されるプロセス (またはプロセスのグループ) に過ぎません。隔離された状態は Linux 名前空間と制御グループによって実現されます。Linux 名前空間と制御グループは Linux カーネルに組み込まれている機能である点に注意してください。Linux カーネル自体を除けば、コンテナーに特殊なところは何もありません。

コンテナーが役立つ理由は、コンテナーを中心に開発されたツールにあります。コンテナーを使用してアプリケーションを構築する際のツールとして事実上の標準となっているのが、このコースのラボで使用する Docker です。Docker には、開発者やオペレーターがあらゆる環境で容易にコンテナーを作成、配布、実行できるよう、ユーザー・フレンドリーなインターフェースが用意されています。

このラボの最初の部分では初めてのコンテナーを実行し、コンテナーを調べる方法を学びます。コンテナーを調べると、Linux カーネルの名前空間によってコンテナーが隔離されていることを確認できます。

初めてのコンテナーを実行した後は、Docker コンテナーのさまざまな使い方を探ります。Docker Store に用意されている多数のサンプル・コンテナーのうち、何種類かのコンテナーを同じホスト上で実行します。こうすることで、隔離によってもたらされるメリット、つまり同じホスト上で複数のコンテナーを実行しても競合が発生しないことを確認できます。

このラボで使用する Docker コマンドは少数に限られています。使用できるコマンドについて詳しくは、[公式のドキュメント](http://docs.docker.jp/)を参照してください。

## 前提条件

ラボ 0 を完了して、Docker がインストール済みになっているか、http://play-with-docker.com を使用できる状態になっていること。


# ステップ 1: 初めてのコンテナーを実行する

Docker CLI を使用して、最初のコンテナーを実行します。 

1. ローカル・コンピューター上でターミナルを開きます。

2. コマンド `docker container run -t ubuntu top` を実行します。

コマンド `docker container run` を使用して、Ubuntu イメージから生成されたコンテナーを実行します。ここでは、`top` コマンドを使用しているので、`-t` フラグで疑似 TTY を割り当てます。`top` コマンドを正常に機能させるためには、こうする必要があります。

```sh
$ docker container run -it ubuntu top
Unable to find image 'ubuntu:latest' locally
latest: Pulling from library/ubuntu
aafe6b5e13de: Pull complete 
0a2b43a72660: Pull complete 
18bdd1e546d2: Pull complete 
8198342c3e05: Pull complete 
f56970a44fd4: Pull complete 
Digest: sha256:f3a61450ae43896c4332bda5e78b453f4a93179045f20c8181043b26b5e79028
Status: Downloaded newer image for ubuntu:latest
```

`docker run` コマンドにより、最初に `docker pull` が実行されて Ubuntu イメージがホストにダウンロードされます。イメージがダウンロードされると、コンテナーが起動されます。実行中のコンテナーからは、次のような出力が生成されます。

```sh
top - 20:32:46 up 3 days, 17:40,  0 users,  load average: 0.00, 0.01, 0.00
Tasks:   1 total,   1 running,   0 sleeping,   0 stopped,   0 zombie
%Cpu(s): 0.0 us,  0.1 sy,  0.0 ni, 99.9 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
KiB Mem :  2046768 total,   173308 free,   117248 used,  1756212 buff/cache
KiB Swap:  1048572 total,  1048572 free,        0 used.1548356 avail Mem 

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND    
    1 root      20   0   36636   3072   2640 R   0.3  0.2   0:00.04 top        

```

`top` は、システム上のプロセスをリソース使用率の順に出力する Linux ユーティリティーです。上記の出力には、プロセスとして `top` プロセス自体しか示されていません。ホストの他のプロセスがこのリストに示されていないのは、PID 名前空間で隔離されているためです。

コンテナーは Linux 名前空間を使用してシステム・リソースを他のコンテナーやホストから隔離します。PID 名前空間により、プロセス ID が隔離されます。コンテナー内部で `top` を実行すると、ホストに対して `top` を実行したときとはまったく異なり、コンテナーの PID 名前空間内にある複数のプロセスが出力に示されるはずです。

ここでは `ubuntu` イメージを使用していますが、このコンテナー専用のカーネルはないことに注意してください。コンテナーはホストのカーネルを使用します。`ubuntu` イメージは、Ubuntu システム上で使用可能なファイル・システムとツールを提供するためだけに使用されています。 

3. `docker container exec` を使用してコンテナーを調べます。

`docker container exec` コマンドは、新しいプロセスによって実行中コンテナーの名前空間に「侵入」する手段です。

新しいターミナルを開きます。play-with-docker.com を使用している場合、node1 に接続された新しいターミナルを開くには、左側にある「Add New Instance (新規インスタンスの追加)」をクリックし、node1 に示されている IP を使用して、node2 から node1 に SSH で接続します。以下に例を示します。

```sh
[node2] (local) root@192.168.0.17 ~
$ ssh 192.168.0.18
[node1] (local) root@192.168.0.18 ~
$ 
```

先ほど作成して実行中になっているコンテナーの ID を取得するために、新しいターミナルで `docker container ls` コマンドを実行します。

```sh
$ docker container ls
CONTAINER ID        IMAGE                      COMMAND                  CREATED             STATUS                         PORTS                       NAMES
b3ad2a23fab3        ubuntu                     "top"                    29 minutes ago      Up 29 minutes                                              goofy_nobel
```



取得した ID を指定して `docker container exec` コマンドを実行することで、コンテナー内部で `bash` を実行します。bash を使用してターミナルからコンテナーとやり取りするため、`-it` フラグを使用します。このフラグにより、疑似ターミナルを割り当てている間は、対話モードを使用して bash を実行できます。

```sh
$ docker container exec -it b3ad2a23fab3 bash 
root@b3ad2a23fab3:/# 
```

成功です！`docker container exec` コマンドを使用して、bash プロセスでコンテナーの名前空間に「侵入」できました。Docker コンテナーを調べる際は、`docker container exec` で `bash` を使用するのが共通パターンとなっています。

ターミナルのプレフィックスが `root@b3ad2a23fab3:/` のように変更されていることに注目してください。これが、コンテナー内で bash を実行していることを示す指標となります。 

**注**: bash プロセスを使用して接続するということは、別個のホストや VM に SSH を使用して接続することとは異なります。bash プロセスで接続する場合、SSH サーバーは必要ありません。コンテナーはカーネル・レベルの機能を使用して隔離を実現すること、コンテナーはカーネルをベースに実行されることを念頭に置いてください。このコンテナーは同じホスト上で隔離されて実行されるプロセスのグループに過ぎません。したがって、この隔離されたグループ内に `bash` プロセスで侵入するには、`docker container exec` を使用できます。`docker container exec` を実行した後は、隔離されて実行されるプロセスのグループに `top` と `bash` が加わります。

同じターミナルから `ps -ef` を実行して、実行中のプロセスを調べましょう。
```sh
root@b3ad2a23fab3:/# ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 20:34 ?        00:00:00 top
root        17     0  0 21:06 ?        00:00:00 bash
root        27    17  0 21:14 ?        00:00:00 ps -ef
```
上記を見るとわかるように、出力に示されるのは、`top` プロセス、`bash` プロセス、`ps` プロセスのみです。

比較のために、コンテナーを終了してホスト上で `ps -ef` または `top` を実行します。これらのコマンドは、Linux 上でも Mac 上でも機能します。Windows の場合、実行中のプロセスを調べるには `tasklist` を使用します。

```sh
root@b3ad2a23fab3:/# exit
exit
$ ps -ef
# Lots of processes!
```

*技術上の詳細* 
PID は、システム・リソースを隔離するために使用できる Linux 名前空間の 1 つに過ぎません。他にも次の Linux 名前空間を使用できます。
- MNT - 他の名前空間に影響することなく、ディレクトリーをマウントまたはアンマウントします。
- NET - コンテナー独自のネットワーク・スタックを使用できます。
- IPC - プロセス間通知メカニズム (メッセージ・キューなど) を隔離します。
- User -システム上のユーザー・ビューを隔離します。
- UTC - コンテナーごとにホスト名とドメイン名を設定します。

以上の名前空間を組み合わせて使用することで、コンテナーのそれぞれを隔離して安全に実行し、同じシステム上で実行されているコンテナー間の競合を回避することができます。次は、コンテナーのさまざまな使い方と、同じホスト上で複数のコンテナーを実行する際に隔離がもたらすメリットについて説明します。

**注**: 名前空間は **Linux** カーネルの機能ですが、Docker を使用すると、Windows 上や Mac 上でもコンテナーを実行できます。それが可能な理由は、Docker 製品には Linux サブシステムが埋め込まれているためです。Docker ではこの Linux サブシステムを [LinuxKit](https://github.com/linuxkit/linuxkit) という新しいプロジェクトとしてオープンソース化しました。さまざまなプラットフォーム上でコンテナーを実行できることが、コンテナーで Docker ツールを使用するメリットの 1 つです。 

Linux サブシステムを使用して Windows 上で Linux コンテナーを実行できるだけでなく、Windows OS に基づくコンテナー・プリミティブが作成されたことから、ネイティブ Windows コンテナーを実行することも可能になっています。ネイティブ Windows コンテナーは、Windows 10 または Windows Server 2016 以降で実行できます。 

4. `top` プロセスを実行中のコンテナーをクリーンアップするために、`<ctrl>-c` と入力します。

# ステップ 2: 複数のコンテナーを実行する

1. Docker Store を調査します。

[Docker Store](https://store.docker.com) は、一般公開された Docker イメージの中央レジストリーです。ここで公開されているイメージは、誰もが共有できます。Docker Store に格納されているコミュニティー・イメージと公式イメージは、[Docker Hub](https://hub.docker.com/explore/) 上で直接検索することもできます。

イメージを検索する際は、「Store」イメージと「コミュニティー」イメージのそれぞれを対象としたフィルターがあります。「Store」イメージに含まれるコンテンツは、Docker によって検証され、セキュリティー上の脆弱性がすでにスキャンされたものです。さらに高度なイメージを見つけるには、「Certified (認定)」イメージを検索します。エンタープライズ対応と見なされるこれらのイメージは、Docker Enterprise Edition 製品でテストされています。重要な点として、本番環境にデプロイする予定の独自のイメージを開発するときは、まだ検証済みでない Docker Store のコンテンツは使用しないようにしてください。検証済みでないイメージには、セキュリティー上の脆弱性が伴っている可能性があります。さらに、悪意のあるソフトウェアである恐れもあります。
 
このラボのステップ 2 では、Docker Store から入手できる検証済みのイメージを使用して、Nginx Web サーバーと Mongo データベースのコンテナーをそれぞれ起動します。

2. Nginx サーバーを実行します。

Docker Store から入手できる[公式の Nginx イメージ](https://store.docker.com/images/nginx)を使用してコンテナーを実行しましょう。

```sh
$ docker container run --detach --publish 8080:80 --name nginx nginx
Unable to find image 'nginx:latest' locally
latest:Pulling from library/nginx
36a46ebd5019: Pull complete 
57168433389f: Pull complete 
332ec8285c50: Pull complete 
Digest: sha256:c15f1fb8fd55c60c72f940a76da76a5fccce2fefa0dd9b17967b9e40b0355316
Status: Downloaded newer image for nginx:latest
5e1bf0e6b926bd73a66f98b3cbe23d04189c16a43d55dd46b8486359f6fdf048
```

上記ではいくつかの新しいフラグを使用しています。そのうちの 1 つ、`--detach` フラグは、このコンテナーをバックグラウンドで実行するためのものです。`publish` フラグでは、コンテナーをホスト上のポート 8080 を介してポート 80 (Nginx のデフォルト・ポート) で公開するように指定しています。前に説明したように、NET 名前空間を使用すると、コンテナー独自のネットワーク・スタックでプロセスを実行できます。`--publish` フラグは、コンテナーを介してネットワークをホストに公開するために使用できる機能です。 

Nginx のデフォルト・ポートがポート 80 であることがわかるのは、Docker Store 上の[ドキュメント](https://store.docker.com/images/nginx)に記載されているためです。一般に、検証済みイメージに付随するドキュメントは非常に優れたものです。検証済みイメージを使用してコンテナーを実行する際は、そのドキュメントを参照することをお勧めします。 

上記では `--name` というフラグも指定しています。これは、コンテナーの名前を指定するフラグです。すべてのコンテナーには名前があります。名前を指定しなければ、Docker がランダムに名前を割り当てます。独自の名前を指定すれば、コマンドでコンテナーの ID ではなく、その名前を参照できるため、コンテナーに対する以降のコマンドを実行しやすくなります。例えば、`docker container inspect 5e1` ではなく `docker container inspect nginx` を使用できます。

Nginx コンテナーを実行するのはこれが初めてなので、Docker Store から Nginx イメージがプルされます。これ以降、Nginx イメージを使用してコンテナーを作成する際は、ホスト上の既存のイメージが使用されます。

Nginx は軽量の Web サーバーです。Nginx サーバーには、ローカル・ホスト上のポート 8080 上でアクセスできます。

3. Http://localhost:8080 上で Nginx サーバーにアクセスします。Play with Docker を使用している場合は、ページの上部にある `8080` リンクを探してください。

![](images/lab1_step2_nginx.png)

4. Mongo DB サーバーを実行します。

次は、MongoDB サーバーを実行します。この場合も、Docker Store から入手できる[公式の MongoDB イメージ](https://store.docker.com/images/mongo)を使用します。ここでは `latest` タグ (タグを指定しない場合のデフォルト) を使用するのではなく、Mongo イメージの特定のバージョン 3.4 を使用します。
```sh
$ docker container run --detach --publish 8081:27017 --name mongo mongo:3.4
Unable to find image 'mongo:3.4' locally
3.4:Pulling from library/mongo
d13d02fa248d: Already exists 
bc8e2652ce92: Pull complete 
3cc856886986: Pull complete 
c319e9ec4517: Pull complete 
b4cbf8808f94: Pull complete 
cb98a53e6676: Pull complete 
f0485050cd8a: Pull complete 
ac36cdc414b3: Pull complete 
61814e3c487b: Pull complete 
523a9f1da6b9: Pull complete 
3b4beaef77a2: Pull complete 
Digest: sha256:d13c897516e497e898c229e2467f4953314b63e48d4990d3215d876ef9d1fc7c
Status: Downloaded newer image for mongo:3.4
d8f614a4969fb1229f538e171850512f10f490cb1a96fca27e4aa89ac082eba5
```

この場合も、Mongo コンテナーを実行するのは初めてのため、Docker Store から Mongo イメージをプルします。`--publish` フラグを使用して、Mongo 用のポートとしてホスト上のポート 27017 を公開します。ホスト上のポート 8080 はすでに公開されているため、ホストのマッピングには 8080 以外のポートを使用する必要があります。Mongo イメージを使用する方法についても、[公式ドキュメント](https://store.docker.com/images/mongo)で詳細を確認してください。

5. http://localhost:8081 にアクセスして、Mongo からの出力を確認します。Play with Docker を使用している場合は、ページの上部にある `8080` リンクを探してください。

![](images/lab1_step2_mongo.png)

6. `docker container ls` を使用して、実行中のコンテナーを確認します。

```sh
$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED                  STATUS              PORTS                     NAMES
d6777df89fea        nginx               "nginx -g 'daemon ..."   Less than a second ago   Up 2 seconds        0.0.0.0:8080->80/tcp      nginx
ead80a0db505        mongo               "docker-entrypoint..."   17 seconds ago           Up 19 seconds       0.0.0.0:8081->27017/tcp   mongo
af549dccd5cf        ubuntu              "top"                    5 minutes ago            Up 5 minutes                                  priceless_kepler
```

Nginx Web サーバー・コンテナーと MongoDB コンテナーがホスト上で実行されていることを確認できるはずです。これらのコンテナーはまだ互いに対話するようには構成していません。

上記の出力には、コンテナーのそれぞれに指定した「nginx」と「mongo」という名前の他に、Ubuntu コンテナーに生成されたランダムな名前 (この例では「priceless_kepler」) が示されています。また、`--publish` フラグで指定したポート・マッピングも確認できます。これらの実行中のコンテナーの詳細について調べるには、`docker container inspect [コンテナー ID]` コマンドを使用します。

お気付きかもしれませんが、Mongo コンテナーは `docker-entrypoint` というコマンドを実行しています。これは、コンテナーの起動時に実行される実行可能ファイルの名前です。Mongo イメージを使用するには、DB プロセスを開始する前にあらかじめ構成を行う必要があります。スクリプトの正確な処理内容を確認するには、[github](https://github.com/docker-library/mongo/blob/master/3.0/docker-entrypoint.sh) 上に置かれているスクリプトを調べてください。一般に、github ソースへのリンクは、Docker Store Web サイト上のイメージの説明ページで見つかります。

コンテナーは自己完結型であり、隔離されています。つまり、異なるシステムやランタイムへの依存関係を持つコンテナー間で発生する可能性のある競合を回避することができます。例えば、Java 7 を使用するアプリと Java 8 を使用する別のアプリを同じホスト上にデプロイする場合でも競合は発生しません。また、デフォルトのリスニング・ポートとして共通してポート 80 を使用する複数の Nginx コンテナーを実行するとしても (`--publish` フラグを使用してホスト上で公開される場合、ホストで選択されているそれぞれのポートは一意でなければなりません)、競合を回避できます。隔離によるこのようなメリットは、Linux 名前空間によってもたらされます。

**注**: 以上のプロセスを実行するためにホスト上にインストールしなければならないものは (Docker の他に) 1 つもありませんでした！各コンテナーに必要な依存関係はそのコンテナーに組み込まれるため、依存関係をホストに直接インストールする必要はまったくありません。

同じホスト上で複数のコンテナーを実行すれば、単一のホスト上で利用可能なリソース (CPU、メモリーなど) をフル活用できます。したがって、企業にとって大幅なコスト削減につながります。

Docker Store のイメージをそのまま実行すると便利な場合もありますが、カスタム・イメージを作成するほうが役立ちます。カスタム・イメージの出発点としては、公式イメージを参照できます。独自のカスタム・イメージを作成する方法については、ラボ 2 で詳しく説明します。

# ステップ 3: クリーンアップ

このラボの手順に従うと、最終的にはホスト上で一連のコンテナーが実行されることになります。これらのコンテナーをクリーンアップしましょう。


1. まず、`docker container ls` を使用して、実行中のコンテナーのリストを取得します。 


```sh
$ docker container ls
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                     NAMES
d6777df89fea        nginx               "nginx -g 'daemon ..."   3 minutes ago       Up 3 minutes        0.0.0.0:8080->80/tcp      nginx
ead80a0db505        mongo               "docker-entrypoint..."   3 minutes ago       Up 3 minutes        0.0.0.0:8081->27017/tcp   mongo
af549dccd5cf        ubuntu              "top"                    8 minutes ago       Up 8 minutes                                  priceless_kepler
```
2. 次に、リストに含まれているコンテナーごとに `docker container stop [コンテナー ID]` を実行します。コンテナー ID ではなく、コンテナーに指定した名前を使用することもできます。
```sh
$ docker container stop d67 ead af5
d67
ead
af5
```

**注**: 参照する必要がある ID は、一意になるだけの桁数があれば十分です。ほとんどの場合は、3 桁の数字があれば一意の ID になります。


2. 停止したコンテナーを削除します。

システムをクリーンアップするには、`docker system prune` コマンドが大いに重宝します。このコマンドは、停止されているコンテナー、未使用のボリュームとネットワーク、孤立したイメージのすべてを削除します。

```sh
$ docker system prune
WARNING!This will remove:
        - all stopped containers
        - all volumes not used by at least one container
        - all networks not used by at least one container
        - all dangling images
Are you sure you want to continue?[y/N] y
Deleted Containers:
7872fd96ea4695795c41150a06067d605f69702dbcb9ce49492c9029f0e1b44b
60abd5ee65b1e2732ddc02b971a86e22de1c1c446dab165462a08b037ef7835c
31617fdd8e5f584c51ce182757e24a1c9620257027665c20be75aa3ab6591740

Total reclaimed space: 12B
```

# まとめ

このラボでは、Ubuntu、Nginx、MongoDB の各コンテナーを初めて作成しました。

このラボで学んだ重要な点
- コンテナーを構成する Linux 名前空間と制御グループにより、コンテナーは他のコンテナーやホストから隔離されます。
- コンテナーは隔離されます。この特性により、依存関係の競合を懸念することなく、単一のホスト上で複数のコンテナーをスケジューリングできます。したがって、単一のホスト上で複数のコンテナーを実行し、そのホストに割り当てられているリソースを簡単にフル活用できるため、最終的にはサーバーにかかる費用を節約できます。
- 独自のイメージを開発する際は、まだ検証されていない Docker Store のコンテンツを使用しないようにしてください。未検証のイメージにはセキュリティー上の脆弱性が伴うことや、悪意のあるソフトウェアである恐れさえあります。
- コンテナー内でプロセスを実行するために必要なものはすべて、そのコンテナーに含まれます。したがって、追加の依存関係をホストに直接インストールする必要は一切ありません。

