

# 組み込みデバイスのベストプラクティス




## プライベートキーの作成



秘密キーをファームウェアに埋め込むことで、誰でもキーを抽出し、それ以外の場合は安全な接続をTCPよりも安全なものに変えることができます。


SSL対応デバイス用のプライベートキーの作成について、いくつかのアイデアがあります。



1. サーバーとして機能する各デバイスは、非組み込みの世界のように、ユニークな秘密鍵を持つ必要があります。


2. 配信前にキーをデバイスに配置できない場合は、セットアップ中に生成されます。


3. セットアップ中にデバイスに独自のキーを生成する機能がない場合は、デバイスをセットアップしているクライアントにキーを生成させ、それをデバイスに送信してもらいます。


4. クライアントに秘密キーを生成する機能がない場合、クライアントに、既知のWebサイト(たとえば)からSSL/TLS接続を介して一意の秘密キーを取得します。



wolfSSL(以前のCyassl)は、これらすべてのステップで使用して、組み込みデバイスに安全な一意の秘密キーを確保するのに役立ちます。これらのステップを踏むことは、SSL接続自体の保護に大いに役立ちます。



## wolfSSL によるデジタル署名と認証



wolfSSLは、組み込みデバイス上でそれらをロードする前に、アプリケーション、ライブラリ、またはファイルにデジタル署名するための一般的なツールです。ほとんどのデスクトップおよびサーバー オペレーティング システムでは、システム ライブラリを介してこのタイプの機能を作成できますが、機能を取り除いた組み込みオペレーティング システムでは作成できません。Embedded RTOS環境にデジタル署名機能が含まれていないため、歴史的にほとんどの組み込みアプリケーションが必要ではなかったためです。Connected Devicesの世界では、セキュリティ上の懸念を高め、埋め込みまたはモバイルデバイスにロードされているものにデジタル署名しています。


この要件が過去に見つからなかった組み込み接続されたデバイスの例には、セットトップボックス、DVR、POIPシステム、VoIPおよび携帯電話、接続ホーム、さらには自動車ベースのコンピューティングシステムが含まれます。wolfSSLは、主要な組み込みおよびリアルタイムオペレーティングシステム、暗号化標準、および認証機能をサポートしているため、デジタル署名機能を追加する際に組み込みシステム開発者が使用することは自然な選択です。


一般に、埋め込みデバイスでコードとファイルの署名を設定するプロセスは次のとおりです。



1. 組み込みシステム開発者はRSAキーペアを生成します。


2. サーバー側のスクリプトベースのツールが開発されています

1.サーバー側のツールは、デバイスにロードされるコードのハッシュを作成します(たとえばSHA-256を使用)。
    2.その後、ハッシュがデジタルで署名され、RSAプライベート暗号化とも呼ばれます。
    3.デジタル署名とともにコードを含むパッケージが作成されます。

3. パッケージは、RSAの公開キーを取得する方法とともに、デバイスにロードされています。ハッシュは、デバイスに再作成され、既存のデジタル署名に対してデジタル検証(RSA public Decryptとも呼ばれます)。



デバイスでデジタル署名を有効にするための利点は次のとおりです。



1. サードパーティがファイルをデバイスにロードできるようにするための安全な方法を簡単に有効にします。


2. 悪意のあるファイルがデバイスに侵入するのを防ぎます。


3. デジタルで安全なファームウェア アップデート


4. 権限のない第三者によるファームウェアの更新を防ぐ



コード署名に関する一般情報：

<https://en.wikipedia.org/wiki/code_signing>