# Xserver でデプロイ

## Xserver に SSH 接続

サーバーパネルよりデータベースを作成
サーバーパネルの SSH 設定を ON にし、公開鍵認証用鍵ペアを生成する
生成した鍵をホームディレクトリの.ssh フォルダに移動
ターミナルも.ssh に移動
cd .ssh
鍵のパーミッションを変更
chmod 700 .ssh
chmod 600 [サーバー ID].key

ssh -l [サーバー ID] -i [サーバー ID].key [サーバー ID].xsrv.jp -p 10022
鍵生成時に設定したパスフレーズを入力すると SSH 接続が完了

## Xserver で laravel の環境構築

### PHP

サーバーパネルで PHP のバージョンをプロジェクトと同じものに設定する
ターミナルで使用する PHP のバージョンもロジェクトと同じものに設定する
mkdir $HOME/bin
ln -s /usr/bin/php8.2 $HOME/bin/php

code ~/.bash_profile
変更前 ⇒
PATH=$PATH:$HOME/bin
変更後 ⇒
PATH=$HOME/bin:$PATH

source ~/.bash_profile

php -v

### コンポーザー

コンポーザーのインストール
下記公式サイトのコマンドを実行
https://getcomposer.org/download/

mv composer.phar $HOME/bin/composer
source ~/.bash_profile
composer -V

### node.js をインストール

wget git.io/nodebrew
perl nodebrew setup

code ~/.bash_profile

bash ファイルに下記を追記し、nodeblrew のパスをとおす
export PATH=$HOME/.nodebrew/current/bin:$PATH

source ~/.bash_profile

nodebrew install v18.0.0
nodebrew use v18.0.0
node -v

## Xserver より guthub に接続する

SSH 接続したターミナルにて、下記のコマンドで鍵を生成
ssh-keygen -t rsa -b 4096
出力先はデフォルトのまま（何も入力せずエンター）
パスフレーズを任意で入力

cat $HOME/.ssh/id_rsa.pub
出力された文字列をコピー
github のプロジェクト用リポジトリの setting、deploy key にペースト。Add Key ボタンをクリック

xserver のターミナルに戻る

プロジェクト用のディレクトリを作成し、github よりクローンする
mkdir pokemon-laravel-xserver
cd pokemon-laravel-xserver
git clone git@github.com:risu043/pokemon-laravel-github.git

プロジェクト直下のディレクトリでコンポーザーをインストール
cd pokemon-laravel-github
composer install

cp .env.example .env
php artisan key:generate

.env ファイルを修正
APP_ENV=production
APP_DEBUG=false
APP_URL=http://[ドメイン]

データベース名、ユーザ名、パスも修正
php artisan migrate

プロジェクト直下で
npm install
npm run build

シンボリックリンク作成
ln -s $HOME/pokemon-laravel-xserver/pokemon-laravel-github/public $HOME/[ドメイン]/public_html
プロジェクト直下でストレージのリンク作成
php artisan storage:link

public_html の.htaccess を修正
<IfModule mod_rewrite.c>
RewriteEngine On

#ssl 通信
RewriteCond %{HTTPS} !on
RewriteRule ^(.\*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

#シンボリックリンク
RewriteCond %{REQUEST_URI} !^/public/
RewriteRule ^(.\*)$ public/$1 [QSA,L]
</IfModule>

完成！

### その他コマンド

フォルダの中身を表示するコマンド(シンボリックリンクを貼れているかの確認に使える)
ls -l
mysql につなぐコマンド
mysql -u [DB ユーザー名] -p -h [DB 名]
直近のエラーログを見るコマンド
tail -n 50 /home/xs350913/pokemon-laravel-xserver/pokemon-laravel-github/storage/logs/laravel.log
