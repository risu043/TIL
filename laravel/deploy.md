# Xserver でデプロイ

## Xserver に SSH 接続

サーバーパネルよりデータベースを作成<br>
サーバーパネルの SSH 設定を ON にし、公開鍵認証用鍵ペアを生成する<br>
生成した鍵をホームディレクトリの.ssh フォルダに移動<br>
ターミナルも.ssh に移動<br>
cd .ssh<br>

鍵のパーミッションを変更<br>
chmod 700 .ssh<br>
chmod 600 [サーバー ID].key

ssh -l [サーバー ID] -i [サーバー ID].key [サーバー ID].xsrv.jp -p 10022<br>
鍵生成時に設定したパスフレーズを入力すると SSH 接続が完了

## Xserver で laravel の環境構築

### PHP

サーバーパネルで PHP のバージョンをプロジェクトと同じものに設定する<br>
ターミナルで使用する PHP のバージョンもプロジェクトと同じものに設定する<br>
mkdir $HOME/bin<br>
ln -s /usr/bin/php8.2 $HOME/bin/php

code ~/.bash_profile<br>
変更前 ⇒<br>
PATH=$PATH:$HOME/bin<br>
変更後 ⇒<br>
PATH=$HOME/bin:$PATH

source ~/.bash_profile

php -v

### コンポーザー

コンポーザーのインストール<br>
下記公式サイトのコマンドを実行<br>
https://getcomposer.org/download/

mv composer.phar $HOME/bin/composer<br>
source ~/.bash_profile<br>
composer -V

### node.js をインストール

wget git.io/nodebrew<br>
perl nodebrew setup

code ~/.bash_profile

bash ファイルに下記を追記し、nodeblrew のパスをとおす<br>
export PATH=$HOME/.nodebrew/current/bin:$PATH

source ~/.bash_profile

nodebrew install v18.0.0<br>
nodebrew use v18.0.0<br>
node -v

## Xserver より guthub に接続する

SSH 接続したターミナルにて、下記のコマンドで鍵を生成<br>
ssh-keygen -t rsa -b 4096<br>
出力先はデフォルトのまま（何も入力せずエンター）<br>
パスフレーズを任意で入力

cat $HOME/.ssh/id_rsa.pub<br>
出力された文字列をコピー<br>
github のプロジェクト用リポジトリの setting、deploy key にペースト。Add Key ボタンをクリック

xserver のターミナルに戻る

プロジェクト用のディレクトリを作成し、github よりクローンする<br>
mkdir pokemon-laravel-xserver<br>
cd pokemon-laravel-xserver<br>
git clone git@github.com:risu043/pokemon-laravel-github.git

プロジェクト直下のディレクトリでコンポーザーをインストール<br>
cd pokemon-laravel-github<br>
composer install

cp .env.example .env<br>
php artisan key:generate

.env ファイルを修正<br>
APP_ENV=production<br>
APP_DEBUG=false<br>
APP_URL=https://[ドメイン]

データベース名、ユーザ名、パスも修正<br>
php artisan migrate

プロジェクト直下で<br>
npm install<br>
npm run build

シンボリックリンク作成<br>
ln -s $HOME/pokemon-laravel-xserver/pokemon-laravel-github/public $HOME/[ドメイン]/public_html

プロジェクト直下でストレージのリンク作成<br>
php artisan storage:link

public_html の.htaccess を修正<br>
<IfModule mod_rewrite.c><br>
RewriteEngine On

#ssl 通信<br>
RewriteCond %{HTTPS} !on<br>
RewriteRule ^(.\*)$ https://%{HTTP_HOST}%{REQUEST_URI} [R=301,L]

#シンボリックリンク<br>
RewriteCond %{REQUEST_URI} !^/public/<br>
RewriteRule ^(.\*)$ public/$1 [QSA,L]<br>
</IfModule>

完成！

### その他コマンド

フォルダの中身を表示するコマンド(シンボリックリンクを貼れているかの確認に使える)<br>
ls -l<br>
mysql につなぐコマンド<br>
mysql -u [DB ユーザー名] -p -h [DB 名]<br>
直近のエラーログを見るコマンド<br>
tail -n 50 /home/xs350913/pokemon-laravel-xserver/pokemon-laravel-github/storage/logs/laravel.log
