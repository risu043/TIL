## 1日

テーブル、モデル、コントローラーの作成

## 2日

postしてもデータベースに登録されない沼

## 3日
コントローラーにLog::infoを設置して、cat storage/logs/laravel.logでエラーログをたどる。<br>
$request->user()->chocos()->create($validated);<br>
上記から下記に変更したらDBに登録されるようになった<br>
$choco = new Choco($validated);<br>
$choco->user_id = $request->user()->id;<br>
$choco->save();<br>

403エラー：コントローラーにGate::authorize('delete', $choco);を入れ忘れてた<br>
500エラー：ポリシーを作成し忘れてた<br>

