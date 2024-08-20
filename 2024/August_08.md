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

## 7日

ブログの機能追加

## 8日

Tanstack Query ... ReactでDBからデータを取得する時の非同期の状態を管理できるライブラリ

useQuery 子コンポーネントでGETリクエストする<br>
QueryClient キャッシュを管理する<br>
QueryClientProvider 親コンポ－ントにて子要素のコンポーネントと QueryClient を接続する<br>
useMutation 子コンポーネントでPOSTリクエストする<br>
invalidateQueries 指定したクエリのキャッシュを無効化する。データ取得時にqueryClientと組み合わせて実行すると再レンダリングされる

## 9日

createContextでプロバイダーを作る<br>
プロバーダーでラップしたコンポーネント内ではuseContextが使用できる<br>
next.jsのthemeProviderに似てる

## 14日

react suspense ... コンポーネントのレンダリング時にデータ取得が必要な場合、データが利用可能になるまでの間フォールバック UI を表示する機能<br>
三項演算子との違い ... コンポーネントからローディングに関わる状態管理が分離されるため、よりシンプルにコンポーネントを記述できる

react error boundary ... エラー時のUIを表示する。suspenseと組み合わせて使用する

## 15日

node.jsのversionを変更する<br>
https://github.com/nvm-sh/nvm#installing-and-updating<br>
nvmを使ってnodeを任意のversionに指定できる。下記コマンドでインストールする
```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
```
~/.bashrc に下記コードが追加されていることを確認する
```
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```
下記コマンドで上記コードを反映させる
```
source ~/.bashrc
```
nodeの最新版をインストールする時
```
nvm install node
```
versionを指定するしてインストールする時
```
nvm install 18.17.0
```
## 16日

先週作ったAPIのフロントを実装しよう

## 17日

typescript コンポーネントに数値を渡したい。数値のみでは渡せないので、プロパティを持つオブジェクトとして渡す
```
<QuantityInput productId={product.id} />
...
function QuantityInput({productId}: {productId: number}): JSX.Element {
...
```
## 18日
オブジェクトのキーのみを取り出し、カンマでつなげて文字列にする
```
const cart: { [key: number]: number } = { 1: 3, 2: 4, 3: 1 };
const productId = Object.keys(cart).join(",");
```
## 19日
#### reduce関数
配列のメソッドで、配列の要素を累積的に処理し、単一の値を返す
```
const totalPrice = products.reduce(
  (sum, product) => sum + (cart[product.id] || 0) * product.price,
  0
);
```
- 第一引数は累積関数です: (sum, product) => ...

- sum: 累積値（この場合は合計金額）
- product: 現在処理中の商品オブジェクト

- 累積関数の中身: sum + (cart[product.id] || 0) * product.price
- cart[product.id]: カート内のその商品の数量を取得
- || 0: カート内にその商品がない場合（undefined）は0とする
- * product.price: 数量に商品の価格を掛ける
- sum + ...: 前の合計に今の商品の小計を加える
- 0: これはreduceの第二引数で、累積の初期値を0に設定しています。
```
const totalQuantity = Object.values(cart).reduce((a, b) => a + b, 0);
```
- Object.values() は、オブジェクトの値だけを取り出して配列にするメソッドです。
- ここでは、cart オブジェクトの全ての値（各商品の数量）を配列にしています。
- 例えば、cart = {1: 2, 2: 1, 3: 3} の場合、[2, 1, 3] という配列が生成されます。
- (a, b) => a + b は累積関数で、現在の合計 a に次の値 b を加えています。
- 0 は初期値で、合計の開始値を0に設定しています。

#### オブジェクトを配列に変換する方法
```
const orders = Object.entries(cart).map(([key, value]) => ({
  productId: Number(key),  // keyは文字列として扱われるため、数値に変換
  quantity: value
}));
```
- Object.entries(cart): cart オブジェクトの各エントリー（キーと値のペア）を配列として取得します。例えば、{1: 5, 19: 5} というオブジェクトは、[[1, 5], [19, 5]] という配列に変換されます。

- map(([key, value]) => ({ ... })): map メソッドで配列をループし、それぞれのキーと値のペア（key, value）から新しいオブジェクト { productId, quantity } を作成します。

- Number(key): キーは元々文字列として扱われるため、数値に変換します。
## 20日
typescript map関数を使用したいのに"プロパティmapは存在しません"のエラーが出る<br>
型の定義で配列を指定し忘れている時に出るエラー<br>
以下のようにArray<>を用いて配列であることを示す
```
export type Order = {
  id: number; // 注文 ID
  userId: number; // ユーザー ID
  createdAt: string; // 注文の作成日時
  updatedAt: string; // 注文の更新日時
  orderDetails: Array<{
    id: number; // 注文詳細 ID
    productId: number; // 商品 ID
    orderId: number; // 注文 ID
    price: number; // 注文時の商品の価格
    quantity: number; // 注文した商品の数量
    createdAt: string; // 注文詳細の作成日時
    updatedAt: string; // 注文詳細の更新日時
    product: Product;
  }>;
};
```

#### 配列の個数による条件分岐
```
selectedOrder.length === 0
```
null や undefined には length プロパティがないので、selectedOrderがnull・undefinedの場合はエラーがおこる
lengthをもちいて条件分岐する時は、下記のようにnullまたはundefinedでないかチェックする<br>
```
selectedOrder && selectedOrder.length === 0
```
