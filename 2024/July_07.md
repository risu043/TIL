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

hyperformでお問い合わせフォームをつくろう

## 27日

react hook form クライアントサイドで動作するライブラリ<br>
page.tsxではなくcomponentでインポートする

React Hook Form は、ref を使用してフォームの入力要素の現在の値を収集する<br>
inputをコンポーネントにする時はReact.forwardRef を使用して定義する必要がある<br>
React.forwardRef を使用する場合、ref の型を適切に定義する
```
'use client';

import { InputHTMLAttributes, forwardRef } from 'react';

export default forwardRef<
  HTMLInputElement,
  InputHTMLAttributes<HTMLInputElement>
>(function TextInput(
  {
    type = '',
    className = '',
    ...props
  }: InputHTMLAttributes<HTMLInputElement>,
  ref
) {
  return (
    <input
      {...props}
      type={type}
      className={
        'w-full p-2 border-gray-300 focus:border-indigo-500 focus:ring-indigo-500 rounded-md shadow-sm ' +
        className
      }
      ref={ref}
    />
  );
});
```

## 28日

デバッグの記録<br>
corsエラー：next.config.mjsにlocalhostからのリクエストを許可するよう追記<br>
404エラー：apiディレクトリ内のスペルミス<br>
405エラー：api/post/route.ts内の関数をexport async function POSTにする<br>
505エラー：fetch先の記述ミス。formのaction属性で送信するときとAPIで送信する時でエンドポイントが異なる
- フォーム https://hyperform.jp/api/{your-form-id}
- API https://hyperform.jp/api/async/{your-form-id}/complete

## 29日

react-hot-toast<br>
フラッシュメッセージを簡易に実装できるライブラリ

fetchが失敗した時にエラーメッセージを表示させたい<br>
fetchのresponseをチェックしてからtoast.successを実行する<br>
response.ok がfalseのときに明示的にエラーをスローしなければ、エラーハンドリングが実行されない<br>
つまりtry & catchのcatchブロックを実行するにはエラーのスローが必要
```
const onSubmit: SubmitHandler<Inputs> = async (data) => {
    try {
      const response = await fetch('/api/postpost', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(data),
      });

      if (response.ok) {
        toast.success('送信が完了いたしました！');
        reset();
      } else {
        throw new Error('送信時にエラーが発生しました');
      }
    } catch (error) {
      toast.error('送信時にエラーが発生しました');
      console.log('error', error);
    }
  };
```
## 30日

Error: Invalid header found<br>
無効なヘッダーに起因するビルドエラー<br>
アプリケーションの設定や依存関係に問題がある場合に発生<br>
node_modules、package-lock.jsonをはずしnpm installで解消

microCMSのブログの特定の記事のみvercelでビルドエラーが生じる<br>
SyntaxError: Invalid regular expression: /\0[oO](([0-7]_*)+)\/mu: Invalid escape<br>
ログには正規表現の無効なエスケープシーケンスに起因するエラーと表示される

microCMSでは未入力のフィールドを含む記事においてビルドエラーが生じる？<br>
記事内のコードブロックのファイル名、言語のフィールドを全て埋めるた後、リデプロイが成功した

## 31日

APIをつくろう
