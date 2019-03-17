
# TLDR
1. `npm install -save-dev gitbook-cli gh-pages` gitbokとgh-pagesをインストールする
2. package.jsonを[用意](https://morizyun.github.io/blog/gitbook-github-pages-deploy/index.html#GitHub-PagesへのPush)
3.  リポジトリで`gitbook init` を実行する
4.  `gitbook serve` で出来栄えを確認
5.  `gitbook build && gh-pages -d _book` を実行して完成
6.  リポジトリの設定から`gh-pages`になっていることを確認する。＋そのあたりに書いているURLからpublishされたページを確認する

以下は完全に自分の作業のログになります。
# 環境構築編
https://casualdevelopers.com/tech-tips/how-to-publish-gitbook-documents-with-github-pages/

ここを見ればたいていのことを書いているのですが、一応補足を。
gitbookについて

https://docs.gitbook.com/

公式のドキュメントはgitbookにポスティングする前提のドキュメントの書き方なので正直微妙。そもそも、今回の目的に合致しない。プライベート1つパブリック1つと絞られるため、もし別のことをしたいとなるとアカウント作り直しとか、契約する必要があるとかになるためなしの方向で。

と思って、githubを見ているとどうやら、ローカルのCLIをアクティブにサポートする気はほぼないみたいである。~~まあ、そりゃ稼げるほうを優先するわな~~。今回は遊び程度なので、一旦こちらでやってみる。Docusaurusでも同じようなことができそうだが、どっちがいいんだろうね？
https://github.com/GitbookIO/gitbook


## checkout --orphan 
しれっと、 `git checkout --orphan gh-page` しているがそれこれなに？って気分になっている。 `orphan` と言われたときに出てきたのが、FF13のラスボスである。あれも孤児という意味だったはずだから、実際にこれもそんな感じなのかなと思ってぐぐった。

https://qiita.com/akiko-pusu/items/7c0a99b8cb37882d2cfe

なるほど。孤児を意図的につくりだして、ドキュメント用のブランチを作るのね。
ただ、このページ通りにpushしてもうまく動かない...原因を調査しよう。

## 調査
最初のindex自体はうまく行っているっぽい。Readmeに書いていることは表示される。
ただ、横のタブが表示されないなど、設定がそもそも間違っている説があるんじゃないかなーと。
一連の操作を見直してみる.

* `gitbook build` これは特に問題のあるコマンドじゃない
* `git checkout --orphan gh-page`  これも問題のあるコマンドじゃない。
* `git subtree push --prefix _book origin gh-pages` これがよくわからないまま実行している感じがする

### git subtree
今までに一度も使ったことがない。少なくとも自分が必要であると感じた瞬間がない。そのため、これはなんぞやということで調査してみる。

https://qiita.com/kyanro@github/items/5520c253c127e576c0ed

なるほど、色々便利そうじゃないか。正しく理解していないと使えなさそうだけど...  
このコマンド自体も大きな問題はなさそう。なら何が問題なのか。

## package.json
このあたり、問題の切り出し方が悪かった気がする

https://morizyun.github.io/blog/gitbook-github-pages-deploy/index.html


ここを参考にpacage.jsonに色々書いて試してみる。これもうまくいかない。
ちなみに、npm packageを使ったほうがいいと思うので後で手順まとめます。

## github page
github pageの設定をそういえばリポジトリを作ったときにいじったような気がする。あと、仕様ってどうなってんだろ？って思って調べてみる。

https://help.github.com/en/articles/configuring-a-publishing-source-for-github-pages

おいおい、`master` が選ばれていたということはmaster直下の`readme.md` を表示するだけになっていたんか。。。？
やばすぎワロタってなったのは言うまでもない...ということで設定変えると表示されましたとさ。

# 反省点
gitbookを使うこと自体が正しいのかどうか。現状gitbook-cliはほぼ開発が止まっているため、使うなら `Docusaurus`
ではという感情が湧いてくる。
