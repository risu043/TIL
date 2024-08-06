## 1日

APIをつくろう<br>
expressルーティングの復習

## 2日

prismaの使い方確認

## 3日

検索API実装の流れ
- queryで検索ワードを取得
- 検索ワードに合うデータをデータベースから取得する
- res.jsonメソッドでresponseとして取得データを返す

```
const filter = req.query["filter"] ? String(req.query["filter"]) : "";
const page = req.query["page"] ? Number(req.query["page"]) : 1;
```
queryには三項演算子を用いて初期値を設定する<br>
データ取得時にqueryの有無による条件分岐は不要である<br>
取得したqueryはstringなので、数字はnumberに変換する

## 4日

POSTするAPI実装<br>
作成したエンドポイントの挙動の確認はブラウザではなくターミナルでおこなう
```
curl -X POST -H "Content-Type: application/json" -d '[]' http://localhost:8000/api/pass
```
## 5日

express-validatorの使い方確認

## 6日

他のGETするAPI実装<br>
validationの順番やparamsの型がテストの判定に関わるらしい
