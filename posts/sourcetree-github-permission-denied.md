---
title: 'SourcetreeでGitHubリポジトリへのpushがpermission deniedになる'
date: '2021-12-17'
---

2021年8月13日以降、GitHubでのパスワード認証が廃止されています。
SourcetreeでGitHubリポジトリをリモート登録してhttps通信でpushしようとしてエラーになる場合、ウェブで症例をググると下記のようにOAuth認証をすれば良いという記事がいくつかあります。
- Sourcetreeメニュー > 環境設定 > アカウントタブ > 追加
- 認証タイプはOAuth、httpsプロトコルを指定し、アカウントを接続
- ブラウザでGitHubのウェブサイトに繋がるので、画面上でAcceptする
- Sourcetreeウィンドウ上で新規アカウントとして無事追加される
![Sourcetreeアカウント](/images/sourcetree_account.png)

これで再度pushを試みるも、私の場合はまだpermission deniedエラーが表示されてpushに失敗しました。
どうやらGitHub上でアクセストークンを作る必要があるようです。アクセストークンは[GitHubドキュメント](https://docs.github.com/ja/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)に従えば作成できました。トークンは一度しか表示されないので、作成完了時に画面表示されるトークンは必ず控えておくようにしましょう。

Sourcetreeにて、設定 > リモートタブ > リモートリポジトリのパス のところを、GitHubが指示してくれる
```
https://（ユーザー名）@github.com/（GitHubアカウント名）/（リポジトリ名）.git
```
ではなく、
```
https://（トークン文字列）@github.com/（GitHubアカウント名）/（リポジトリ名）.git
```
とすることでpushできるようになりました。

参考サイト：  
[SourceTreeでGitHubのPersonal access tokensを利用する方法](https://zenn.dev/koushikagawa/articles/3c35e503c8553a)  
ほぼこちらのサイトの通りに従えば良いのですが、私の場合はpush時にパスワードを求めてくれず、トークンを設定する場所が見当たらなかったので  
[SourcetreeでGitHubの認証に失敗した件 | 田舎暮らしエンジニアの備忘録 - 楽天ブログ](https://plaza.rakuten.co.jp/metrodevnote/diary/202108290000/)  
に書かれているように、リモートリポジトリのパスにユーザー名ではなくトークンを含めるようにして解決できました。
