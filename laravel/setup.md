プロジェクト作成
curl -s https://laravel.build/pokemon | bash

作成したプロジェクトのディレクトリに移動
cd pokemon

Sail コマンドを使えるようにする
code ~/.bashrc
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'

変更を反映
source ~/.bashrc

sail を起動
sail up

localhost に laravel が表示されるようになる

config/app.php を修正
'timezone' => env('APP_TIMEZONE', 'Asia/Tokyo'),
'locale' => env('APP_LOCALE', 'ja'),

Laravel Breeze パッケージをインストール
sail composer require laravel/breeze --dev

Laravel Breeze を React-typescript でインストール
sail artisan breeze:install react --typescript

フロントエンドの依存関係をインストール
sail npm install && sail npm run dev
