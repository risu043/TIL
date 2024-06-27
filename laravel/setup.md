## プロジェクト作成
curl -s https://laravel.build/pokemon | bash

作成したプロジェクトのディレクトリに移動<br>
cd pokemon

Sail コマンドを使えるようにする<br>
code ~/.bashrc<br>
alias sail='[ -f sail ] && bash sail || bash vendor/bin/sail'

変更を反映<br>
source ~/.bashrc

sail を起動<br>
sail up

localhost に laravel が表示されるようになる

config/app.php を修正<br>
'timezone' => env('APP_TIMEZONE', 'Asia/Tokyo'),<br>
'locale' => env('APP_LOCALE', 'ja'),

Laravel Breeze パッケージをインストール<br>
sail composer require laravel/breeze --dev

Laravel Breeze を React-typescript でインストール<br>
sail artisan breeze:install react --typescript

フロントエンドの依存関係をインストール<br>
sail npm install && sail npm run dev
