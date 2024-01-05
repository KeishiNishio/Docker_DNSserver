# NSDベースのDNSサーバー

このリポジトリは、NSD (Name Server Daemon) を使用してDNSサーバーを構築するための設定ファイル群です。

### **NSDの選択理由**

従来は、BINDがDNSの実装に広く使用されてきたようですが、BINDは脆弱性が度々発見され、セキュリティ的に運用が難しいことから、以下の観点から優れた代替手段としてNSDを選定しました。

- BINDに比べて、高度なセキュリティ
- 読み取り専用の機能に特化し、大規模な環境でも高速で安定したパフォーマンス
- BINDと比較し、設定がシンプル

参考サイト

[https://qiita.com/ohhara_shiojiri/items/497aaba989151fa84b3d](https://qiita.com/ohhara_shiojiri/items/497aaba989151fa84b3d)

### **システム構成図**

クライアント（ローカルホスト）からの名前解決クエリを受け取り、権威DNSサーバからクライアントにIPアドレスを返送する

![スクリーンショット 2024-01-06 3.38.28.png](NSD%E3%83%98%E3%82%99%E3%83%BC%E3%82%B9%E3%81%AEDNS%E3%82%B5%E3%83%BC%E3%83%8F%E3%82%99%E3%83%BC%20feb77c7960d14ba3ba2afd5331f6249a/%25E3%2582%25B9%25E3%2582%25AF%25E3%2583%25AA%25E3%2583%25BC%25E3%2583%25B3%25E3%2582%25B7%25E3%2583%25A7%25E3%2583%2583%25E3%2583%2588_2024-01-06_3.38.28.png)

## セットアップ手順

1. **リポジトリのクローン**: まず、このリポジトリをローカルマシンにクローンします。
    
    ```
    git clone [このリポジトリのURL]
    
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