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

## 7日

allDay:falseの場合、startとendはUTCで送信される(日本時間と9時間ずれる)<br>
controllerでvalidationした後、Carbon(日付操作ライブラリ)で日本時間に戻してからDB登録する

eventInputのうちendはnullableである(時間指定のイベントで、かつ期間が１時間の場合)<br>
作成したイベントの期間を変更したい場合は、Fullcallendarのpropsにてdroppableの他にeventResizableFromStartの定義が必要<br>
handleEventDrop関数に加えhandleEventResize関数を用意する(ルーティングは共用で可)

## 8日

カレンダー完成。xserverにデプロイした

## 9日

tone.jsのドキュメント

## 10日

react-piano作成、デプロイ

## 11日

react-pianoのCSS修正、safari崩れを解消

## 12日

next.js、microCMSでブログをつくろう

## 13日

```
<Link href={`/blogs/${blog.id}`}>
```
blogsフォルダ内に[bogId]フォルダを作成し、さらにpage.tsxを作成する

## 14日

```
const data = await getList({
    filters: `category[equals]${categoryId}`,
  });
```
category別しぼりこみ機能。fetch時にフィルタリングする

## 15日

今いるページのページネーションにクラス名curruntを付与する<br>
親コンポーネント（ページネーションリンク先ページ）から今いるページのidをpropsとして渡し、map関数内で比較する<br>
idの初期値は1としておく
```
{range(1, Math.ceil(totalCount / PER_PAGE)).map((number) => (
        <li key={number}>
          {current !== number ? (
            <Link
              href={`/blogs/page/${number}`}
              className=""
            >
              {number}
            </Link>
          ) : (
            <span className="currunt">
              {number}
            </span>
          )}
        </li>
      ))}
```

## 16日

リッチエディタで入力させたコードブロックを表示させる<br>
ライブラリとその型をインストール
```
npm install cheerio highlight.js
npm install --save-dev @types/cheerio
```

formatRichText関数を作る
- 引数として受け取ったHTMLをCheerioライブラリを使ってロード
- HTML内のすべてのcodeブロックを見つけ、highlight.jsライブラリを用いてハイライト処理
- ハイライトされたHTML全体を返す
  
記事ページのcontent部分に上記関数を使用する

## 17日

vercelのアカウント作成、ブログをデプロイ<br>
.envに書いてたSERVICE_DOMAINとAPI_KEYはsettingのEnvironment Variablesに入力する

## 18日

ビルドエラーの解消

## 19日

next-themesのインストール<br>
ThemeProviderコンポーネントを作成し、アプリケーション全体を正しくラップする（Layout.tsxのbodyの中身ぜんぶ）<br>
cssはglobalcssに以下のように記述
```
html[data-theme='light'] {
  background-color: black;
  color: white;
}
html[data-theme='dark'] {
  background-color: white;
  color: black;
}
```

## 20日

input type="checkbox"を使ってテーマを切り替えるコンポーネントを作成<br>
アニメーションするボタンはinputの疑似要素にtransitionを指定して作る

## 21日

preタグにwidthとoverflow-x:autoを設定して、画面幅からはみ出ないようにした<br>
layout.tsxに設置していたOGPをはずし各page.tsxに設置<br>
ブログ詳細ページのOGPにはfetchで取得したデータを入れて動的にした

## 22日

クライアントコンポーネントでfetchするとエラーになる<br>
サーバーコンポーネントでfetchし、propsとして各コンポーネントに渡す

## 23日

検索機能実装<br>
検索バーのクライアントコンポーネントにてuseSearchParamsを使って検索ワードを取得<br>
useRouterを使ってsearchParamsを含むURLへと遷移させる<br>
遷移先のページにて、searchParamsを用いて必要なデータをfilteringしながらfetchする<br>
検索バーコンポーネントは必ずsuspenseでラップする

## 24日

シェアボタン実装<br>
xのサポートページから実装用のコードを取得<br>
fetchで取得したデータを格納すると動的にできる

priority={true}<br>
ページ上部に位置する画像(Largest Contentful Paint)に設定する<br>
画像の読み込みの優先度を明し、next/imageパフォーマンスを向上させるため

## 26日

heperformでお問い合わせフォームをつくろう
