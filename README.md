# 環境構築方法
## 構築を行う前の確認事項
1. 自身のPC上に`composer` `vue.js` `php`が導入されているか確認。
2. `XAMPP`もしくは`MAMP`が導入されているか確認
3. その上で`XAMPP`もしくは`MAMP`上の`htdocs`ディレクトリをルートディレクトリとしているか確認。
4. `XAMPP`もしくは`MAMP`上のサーバーを起動
5.  `next.js:^13.0.3` `Laravel:11.4.0`に対応した構築となるので注意。おそらくメジャーアップデートが発生すると、構築できない恐れあり・・・
## バックエンド
1.Laravelの最新の安定版をダウンロード
```
composer create-project --prefer-dist laravel/laravel "プロジェクト名"
```
```
cd  "プロジェクト名"
```

### 2.Laravel Breezeをインストールする
```
composer require laravel/breeze --dev
```
Laravelフレームワークのための軽量な認証スターターキットで、ログイン、登録、パスワードリセット、メール確認、二要素認証などの基本的な認証機能が追加。

--devオプションを使用すると、パッケージは開発環境でのみ利用可能になり、本番環境では利用できなくなります。開発中にのみ必要なパッケージに対してよく使用される。
### 3.Laravel専用のコマンドである、artisanコマンドを使用しインストールを実行する
```
php artisan breeze:install api
```
Laravel Breezeパッケージをインストールした後に実行するもので、LaravelプロジェクトにBreezeの認証ビューとルーティングを設定。また、Laravel BreezeのAPIサポートをインストールするためのもの。これを行わないと、Next.jsと行き来出来ない。先ほどの`composer require laravel/breeze --dev`コマンドとの違いは、`composer require`コマンドがパッケージをプロジェクトにダウンロードしてインストールするのに対し、こちらはインストールしたパッケージをプロジェクトで使用するための設定を行う。
### 4.DB・言語等の設定を行う
`.env`ファイル内の項目を下記内容に変更する。
因みに、DBはMySQLを使用する形を想定し設定中
```
APP_NAME=Laravel
APP_ENV=local
APP_KEY=base64:H7nsUi2J8aDUSrZVnRbqp0A7gm+UASjAaWll6M+0X9I=
APP_DEBUG=true
APP_TIMEZONE=Asia/Tokyo
APP_URL=http://localhost:8000
FRONTEND_URL=http://localhost:3000

APP_LOCALE=ja
APP_FALLBACK_LOCALE=ja
APP_FAKER_LOCALE=ja_JP

APP_MAINTENANCE_DRIVER=file
APP_MAINTENANCE_STORE=database

BCRYPT_ROUNDS=12

LOG_CHANNEL=stack
LOG_STACK=single
LOG_DEPRECATIONS_CHANNEL=null
LOG_LEVEL=debug

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT="MAMPもしくはXAMPnoポート番号"
DB_DATABASE="任意のDB名称入力"
DB_USERNAME=root
DB_PASSWORD="DBのPASS記載。デフォルトはroot"

SESSION_DRIVER=database
SESSION_LIFETIME=120
SESSION_ENCRYPT=false
SESSION_PATH=/
SESSION_DOMAIN=null

BROADCAST_CONNECTION=log
FILESYSTEM_DISK=local
QUEUE_CONNECTION=database

CACHE_STORE=database
CACHE_PREFIX=

MEMCACHED_HOST=127.0.0.1

REDIS_CLIENT=phpredis
REDIS_HOST=127.0.0.1
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_MAILER=log
MAIL_HOST=127.0.0.1
MAIL_PORT=2525
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
MAIL_FROM_ADDRESS="hello@example.com"
MAIL_FROM_NAME="${APP_NAME}"

AWS_ACCESS_KEY_ID=
AWS_SECRET_ACCESS_KEY=
AWS_DEFAULT_REGION=us-east-1
AWS_BUCKET=
AWS_USE_PATH_STYLE_ENDPOINT=false

VITE_APP_NAME="${APP_NAME}"

```
### 5.データベースのマイグレーションを実行
（データベースのスキーマを定義するための方法。テーブルの作成や変更などをコードで管理する為に実行する）
```
php artisan migrate
```
### 6.日本語の設定を行う
下記コマンドを入力し、プロジェクト直下にlangディレクトリを作成
```
php artisan lang:publish
```
下記２コマンドを後から、入力
```
composer require askdkc/breezejp --dev
```
```
php artisan breezejp
```

下記内容を参照

[Laravelスターターキットについて](https://laravel.com/docs/11.x/starter-kits#breeze-and-next)

[Laravel Breeze 日本語化パッケージ：Breezejp](https://github.com/askdkc/breezejp)
### 7.サーバーを立ち上げる(バックエンド)
```
php artisan serve
```
### 8. 動作確認
Webブラウザを立ち上げ、下記URLを入力し、laravelのverが表示されればOK
```
localhost:8000
```

## フロントエンド
### 1. Next.js フォルダーをcloneする
```
git clone git clone https://github.com/laravel/breeze-next
.git "任意のプロジェクト名。おすすめは<client>"
```
```
cd "任意のプロジェクト名"
```
参照先

[Next.jsスターターキット](https://github.com/laravel/breeze-next)

### 2. pageルーターverのNext.jsをダウンロード
```
 git checkout -b '任意のbranch名' 81ce4caab419aaa3dc5f389a8cfd8afb33143bfa
```

### 3. npmをインストールする
```
npm install
```
### 4. .envファイルを編集する
`.env.example`ファイルをコピーして、`.env.local`ファイルを作成する。

### 5.サーバーを立ち上げる
```
npm run dev
``` 
### 6.動作確認を行う

Webブラウザを立ち上げ、下記URLを入力し、画面が表示されればOK
```
localhost:3000
```
