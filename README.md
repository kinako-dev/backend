# Go(Echo) + MySQL CRUD API サーバー環境構築ガイド

## 前提条件

- Docker がインストールされていること
- Docker Compose が利用可能であること
- Git がインストールされていること（ソースコードのクローン用）

## 環境構築手順

1. **リポジトリのクローン**

ソースコードをローカルにクローンします。

```
git clone https://github.com/kinako-dev/backend.git
cd backend
```

2. **Docker イメージのビルドとコンテナの起動**

Docker Compose を使用して、必要な Docker イメージをビルドし、コンテナを起動します
air 導入済み

```
docker-compose up --build
```

初回のビルドには時間がかかることがあります。コンテナが起動したら、API サーバーが`http://localhost:8080`で利用可能になります。
「db connected!!」の記述があれば成功！

3. **実際にテーブルが作成されているか確認**

新しいターミナルを起動し、以下を実行して db コンテナ内に入ります。

```
docker compose exec db bash
```

bash-〇.〇#と出てくれば成功です。

次に、以下をを実行します。

```
mysql -u root -p
```

Enter password:と出てくるので、mysql/.env に記述した MYSQL_ROOT_PASSWORD を打って Enter を押します。

以下のように出れば MySQL に接続できます。

```
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 9
Server version: 8.0.35 MySQL Community Server - GPL

Copyright (c) 2000, 2023, Oracle and/or its affiliates.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql>
```

ここで、以下コマンドを実行し使用するデータベースを決めます。

```
use test_database;
```

最後に、以下コマンドを実行して users テーブルが作成されているか確認します。

```
show tables;
```

```
mysql> show tables;
+-------------------------+
| Tables_in_test_database |
+-------------------------+
| users                   |
+-------------------------+
1 row in set (0.06 sec)
```

4. **API のテスト**

API が正しく機能しているかを確認するために、curl や Postman を使用してテストリクエストを行います。

```
curl http://localhost:8080/users
```

## トラブルシューティング

- **docker compose up コマンドを実行した時**
  network golang_test_network declared as external, but could not be found と出た場合は、以下コマンドを実行してネットワークの生成を行います。

```
docker network create golang_test_network
```

- **コンテナが起動しない場合**

Docker Compose のログを確認して、エラーの原因を特定します。

```
docker-compose logs
```

- **データベース接続エラー**

`.env`ファイルの設定を確認し、正しいデータベース資格情報が設定されていることを確認してください。

## その他

この環境は開発用です。本番環境にデプロイする場合は、セキュリティ設定やパフォーマンスの最適化が必要です。

## mysql/.env

```
MYSQL_DATABASE=database
MYSQL_USER=user
MYSQL_PASSWORD=password
MYSQL_ROOT_PASSWORD=root_password
```
