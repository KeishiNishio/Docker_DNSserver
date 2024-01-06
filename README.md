# NSDベースのDNSサーバー

このリポジトリは、DockerコンテナにNSD (Name Server Daemon) を使用してDNSサーバーを構築するための設定ファイル群です。

### **NSDの選択理由**

従来は、BINDがDNSの実装に広く使用されてきたようですが、BINDは脆弱性が度々発見され、セキュリティ的に運用が難しいことから、以下の観点から優れた代替手段としてNSDを選定しました。

- BINDに比べて、高度なセキュリティ
- 読み取り専用の機能に特化し、大規模な環境でも高速で安定したパフォーマンス
- BINDと比較し、設定がシンプル

参考サイト

[https://qiita.com/ohhara_shiojiri/items/497aaba989151fa84b3d](https://qiita.com/ohhara_shiojiri/items/497aaba989151fa84b3d)

### **システム構成図**

クライアント（ローカルホスト）からの名前解決クエリを受け取り、権威DNSサーバからクライアントにIPアドレスを返送する

![システム構成図](https://github.com/KeishiNishio/Docker_DNSserver/blob/main/%20systemimage1.png)

## セットアップ手順

1. **リポジトリのクローン**: まず、このリポジトリをローカルマシンにクローンします。
    
    ```
    git clone https://github.com/KeishiNishio/Docker_DNSserver.git
    
    ```
    
2. **Dockerイメージのビルド**: 次に、提供された`Dockerfile`を使用してDockerイメージをビルドします。
    
    ```
    docker-compose build
    ```
    
3. **Dockerコンテナの起動**: 以下のコマンドでDockerコンテナを起動します。
    
    ```
    docker-compose up -d
    ```
    

これで、NSDベースのDNSサーバーが起動します。

### 挙動の確認

以下のコマンドを実行すると、対応するIPアドレスが返送されます。

```bash
dig @localhost example.com
```

## ファイル構成

- `docker-compose.yml`: NSDサービスのDocker設定を含んでいます。
- `Dockerfile`: NSDのインストールと設定ファイルの配置を指示します。
- `nsd.conf`: NSDの設定ファイルです。
- `zonefile.zone`: `example.com`ドメインのDNSゾーンファイルです。

## 他便利なコマンド

1. システム全体のDockerキャッシュを削除

```bash
docker system prune --all --force
```

1. 設定したネットワークにコンテナが所属しているか確認

```bash
docker network inspect nginx-network
```

1. コンテナの状況確認

```bash
docker ps -a
```

1. 存在する全てのコンテナを停止

```bash
docker stop $(docker ps -aq) || true
```

1. 存在する全てのコンテナを削除

```bash
docker rm -f $(docker ps -aq) || true
```


## 開発環境

このプロジェクトは以下の環境で開発されています:

- **オペレーティングシステム**: macOS
- **Docker**:
  - バージョン: 24.0.5
- **Docker Compose**:
  - バージョン: 2.23.3
