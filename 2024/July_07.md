## 1日

テーブル、モデル、コントローラーの作成

## 2日

postしてもデータベースに登録されない沼

## 3日
コントローラーにLog::infoを設置して、cat storage/logs/laravel.logでエラーログをたどる。<br>
403エラー：コントローラーにGate::authorize('delete', $choco);を入れ忘れてた<br>
500エラー：ポリシーを作成し忘れてた<br>

