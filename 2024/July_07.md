## 1日

テーブル、モデル、コントローラーの作成

## 2日

postしてもデータベースに登録されない沼

## 3日
コントローラーにLog::infoを設置して、cat storage/logs/laravel.logでエラーログをたどる

## 4日

eventの登録、DBから取得したeventのレンダリングまでできた<br>
postのステータスコードが200でも、controllerのバリデーションにひっかかるとDBに登録されない

## 5日

DBからeventを削除できた<br>
FullCalendarのeventオブジェクトのidはstring、DBからfetchしたidはnumber<br>
fetch後はtoStringメソッドでstringにそろえる

## 6日

PATCHでデータを送ろうとすると405エラーがでる<br>
HTMLがGETとPOSTしかサポートしていないため<br>
method="POST"、bodyに_method="patch"を加え送信する

laravelはbooleanを整数に変換してDBに保存する(true:1,false:0)<br>
Modelに下記のようにメソッドを追加することで、もとの形式でfetchできる<br>
protected $casts = [
        'allDay' => 'boolean',
    ];
