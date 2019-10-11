---
title: 最近話題のヘッドレスCMS「microCMS」+Nuxtでサイトを作った話
tags: nuxt.js microCMS Vue.js CMS HeadlessCMS
author: yutopia898
slide: false
---
microCMS+Nuxt+Netlifyでサイトを作りました。
<blockquote class="twitter-tweet"><p lang="ja" dir="ltr">new shitです。みてくれたらうれしいです。。<a href="https://t.co/8pouvxCCq8">https://t.co/8pouvxCCq8</a></p>&mdash; ゆ (@uchoco898) <a href="https://twitter.com/uchoco898/status/1181396482898817024?ref_src=twsrc%5Etfw">October 8, 2019</a></blockquote> <script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
twitterはじめたてでエンジニアさんをフォローしまくりたいのでよかったらフォローして欲しいです。
##使用技術
・Nuxt.js
・microCMS
・Netlify
のJAMstack構成です。
##この記事の目的
僕はWordPressが嫌いです。ついでに言うとJQueryも使いたくないです。でも実務ではまだまだ使われているのが現状なのではないでしょうか？僕は使ってますよ（T T）
microCMSを触ってみて、クライアントワークでもreactとかvue+ヘッドレスCMSがWordPressに取って代われる可能性を大いに感じたので、それを少しでも応援したいと思い書きました。僕は普段web制作会社でアルバイトしているのでweb制作会社のクライアントワーク視点になっていると思います。
##microCMSのいいところ
###日本製！全てが日本語！！！
他にヘッドレスCMSというとContentfulなどが有名だと思いますが、特にクライアントにコンテンツの更新作業をしてもらう場合など、英語の管理画面を触ってもらうのは気が引けます。その点でmicroCMSは日本人が使うハードルが他のヘッドレスCMSに比べて圧倒的にハードルが低いです。これだけでも使う理由になると思います。
###UIがわかりやすい
そしてmicroCMSはUIがとてもモダンですっきりとしています。
<img width="1405" alt="スクリーンショット 2019-10-09 3.42.45.png" src="https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/472897/549e0422-9ca6-ab8d-8d57-3f2a90ceef26.png">

こんな感じでどこを触ればどのようなことが起きるのか想像でき、とてもわかりやすい構成だと思います。
###クライアントに必要な部分だけを見せられる
少しUIがわかりやすいと被りますが、microCMSは管理画面上でクライアントに必要な部分（更新可能なコンテンツ部分）だけを見せて、不必要な部分（開発側の設定など）が見えにくい構成になっているなと感じます。これの何が素晴らしいかというと、例えばWordPressでは、管理画面のサイドバーからテーマを変えることもできれば、パーマリンクの設定を変えることもできてしまいます、プラグインを導入すると、その設定項目も管理画面に表示されます。そして、運用していく上でそれらを不用意に変えてしまい、表示部分がおかしくなってしまったりというのはよくある話ではないでしょうか？このようなリスクを低くできますし、単純にwebサイトを商品とした時の完成度も高くなると思います。
開発ロードマップにも「開発者と編集者で表示するメニューを変える」とあるのでその辺を意識して開発されているのだなと思います。
###月100GBまで無料
Netlifyに静的ホスティングしていて一週間でだいたい0.05GBぐらい使用しました。これに関してはプロジェクトによりそれぞれだと思いますが、僕は１GBも行かないと思います。
###どんどん使いやすく改善されている
開発スピードがとても早く、こんな記事を書いている今日も管理画面上のドラッグ＆ドロップで記事の並べ替えができるようになりました。開発している会社様の熱意が伝わってきてずっと使います！！という気持ちになります。
##技術的なところも少しだけ書いてみる
Nuxt.jsとヘッドレスCMSを使ったサイトの構築はNuxt.jsとContentfulでサイトを作ってみた系の記事がたくさんあるのでそれらを読めばなんとかなるかと思いまので、Nuxt.jsにおけるmicroCMSでのデータの取得部分のコードを載せておこうと思います。
超経験豊富なエンジニアではないので参考にならないかもしれませんが、少しでも導入の助けになれば幸いです。
###データの取得部分

```js:index.vue
export default = {
....
async asyncData({env}) {
    const myHttpClient = axios.create({
      baseURL: 'https://hogehoge.microcms.io/api/v1/'//自分が設定したドメイン.microcms.io/api/v1/です
      headers: {
        'X-API-KEY': 'xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxx'//与えられるAPI-KEY
      }
    })
    const [Object1, Object2, Object3, Object4] = await Promise.all([
      //https://hogehoge.microcms.io/api/v1/endpoint1~4からデータを取ってくる。
      myHttpClient.get('endpoint1'),//ここはmicrocmsで設定したエンドポイント名です。
      myHttpClient.get('endpoint2'),
      myHttpClient.get('endpoint3'),
      myHttpClient.get('endpoint4')
    ])
    return { Object1, Object2, Object3, Object4 }
  },
.....
}
```

これでdataに情報が格納されるので、あとは煮るなり焼くなり好きにできます。

nuxt generateすると同じだと思いますが、API-KEYを直接打ち込まず、環境変数に置き換えるにはこちらの記事が参考になります。
[Nuxt.jsにおけるenvファイルの利用(初学者向けハンズオン)](https://qiita.com/yfujii1127/items/c77bff6f0177b4ff219e)
これを参考に書き換えると

```:.env
API_KEY = xxxxx-xxxxxxx-xxxxxx-xxxxx
baseUrl= https://hogehoge.microcms.io/api/v1
```
余談ですが、僕は.envファイルのAPI_KEYとbaseUrlにずっと""をつけていて2時間ぐらい悩んでました。

```js:nuxt.config.js
require('dotenv').config()
const { API_KEY, baseUrl } = process.env
module.exports = {
....
 env: {
    API_KEY: process.env.API_KEY,
    baseUrl
  },
....
}
```
###データの取得部分(環境変数導入ver)

```js:index.vue
export default = {
....
async asyncData({env}) {
    const myHttpClient = axios.create({
      baseURL: env.baseUrl//env.設定した名前　です
      headers: {
        'X-API-KEY':env.API_KEY//env.設定した名前　です
      }
    })
    const [Object1, Object2, Object3, Object4] = await Promise.all([
      //https://hogehoge.microcms.io/api/v1/endpoint1~4からデータを取ってくる。
      myHttpClient.get('endpoint1'),//ここはmicrocmsで設定したエンドポイント名です。
      myHttpClient.get('endpoint2'),
      myHttpClient.get('endpoint3'),
      myHttpClient.get('endpoint4')
    ])
    return { Object1, Object2, Object3, Object4 }
  },
.....
```
となります。

##まとめ
以上microCMSを応援する記事でした。
無料で使えますし、とりあえず触ってみることをオススメします！

######PS
基本ReactとかVueとか使って制作してる制作会社さんありませんか？
あったらお話聞きたいです。
