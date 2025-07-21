---
layout: post
title: Claude Code GitHub Actionsでアプリ開発を試した
date: 2025-07-21 20:08 +0900
---

最近 2 ヶ月くらいは Agent Coding に主に Claude Code を使っている。Cline/Roo Code, Cursor, Devin あたりを少し使いつつ、メインは Claude Code になっている。感覚値でしかないが、自分が意図した結果を出力してくれる確率が高いことが理由だ。 CLI ベースなので、gh や rg などの CLI コマンドとも連携させやすいし、ターミナルから直接起動できるので画面を広く使えるし、並列で使うことも簡単にできるので、使い勝手がいい。

なので普段はターミナル(iTerm2)から使っているのだが、[Claude Code GitHub Actions](https://docs.anthropic.com/en/docs/claude-code/github-actions)の存在が気になっていながら試せていなかったので、ウェブアプリを新しく作ってその GitHub レポジトリに Claude Code GitHub Actions を組み込んで少し開発してみた。

手元にある本の ISBN を OCR で読み取って本の情報を取得して共有したりコピペできるアプリを作り始めた。

もともとの動機としては、ごくシンプルな本の貸出管理アプリを作りたいと思っていた。共有の本棚があったときに、そこから「この本借ります」「返しました」みたいに Slack でやりとりされているのを見て、手元にある本をカメラで取ってそのまま Slack にあげられたら便利ではないかと思った。

まだ貸出管理データベースの仕組みまでは作ってないが、 作ったウェブアプリは Vercel にデプロイしてある。

[https://isbn-reader.vercel.app/](https://isbn-reader.vercel.app/)

カメラを使うことを前提としているのでスマートフォンから使うことを想定した UI になっている。

![app image]({{site.baseurl}}/assets/images/2025-07-21-01.png)
![app image]({{site.baseurl}}/assets/images/2025-07-21-02.png)
![app image]({{site.baseurl}}/assets/images/2025-07-21-03.png)

アプリとしてはまだまだ完成度が低いが、ひとまず今日のところはClaude Code GitHub Actionsをある程度実用的なサンプル上で使ってみるのが主目的。

GitHub レポジトリはこちら。

[chokkoyamada/isbn-reader: 📚 ISBN 読み取り本情報共有システム - TDD 方式で開発された Next.js + OCR + カメラ撮影対応アプリ](https://github.com/chokkoyamada/isbn-reader)

まずベースを手元(Mac のローカル + VSCode)で作って Push して、途中から Claude Code GitHub Actions を組み込んだ。

導入の手順としては、[公式ドキュメント](https://docs.anthropic.com/en/docs/claude-code/github-actions)に沿って `/install-github-app` を実行しただけだ。これによってレポジトリの Secret として `ANTHROPIC_API_KEY` が作られて、必要な workflow ファイルが作られて、pull request を作る直前でブラウザが起動する。Pull Requestを作るのは自分自身になる。

作った pull request が下記。

[Add Claude Code GitHub Workflow by chokkoyamada · Pull Request #1 · chokkoyamada/isbn-reader](https://github.com/chokkoyamada/isbn-reader/pull/1)

これをマージすると Claude Code GitHub Actions が使えるようになる。

なお、Claude Codeは個人開発用途としてはMaxプラン等には入っておらず、API Keyで都度チャージして利用している。怖いのでオートリロードはオフにしている。現時点で残高は$30くらいある。最近個人開発の時間もあまり取れてないので、月間$100も消費しないので、都度チャージで十分だ。

このあとはGitHubのIssueにやってほしいことを書いていく。

下記の4つのIssueを作って、それぞれでpull requestを作ってマージした。

[バーコード読み取り機能の追加 · Issue #2 · chokkoyamada/isbn-reader](https://github.com/chokkoyamada/isbn-reader/issues/2)

[Web Share API を使用して Slack に共有できるボタンの実装 · Issue #3 · chokkoyamada/isbn-reader](https://github.com/chokkoyamada/isbn-reader/issues/3)

[共有ボタンの整理・統合 · Issue #6 · chokkoyamada/isbn-reader](https://github.com/chokkoyamada/isbn-reader/issues/6)

[「画像をアップロード」機能の削除 · Issue #7 · chokkoyamada/isbn-reader](https://github.com/chokkoyamada/isbn-reader/issues/7)

pull request を 4 つ作ってかかったコストは$2.2 くらいだった。

以下気になった点。

- pull requestはClaude Code自身で自動では作ってくれない？ Remote BranchへのCommit & Pushまではしてくれるが、pull requestは自分で作る必要があった。

- なんかパーミション周りで失敗してGitHub Actionsのジョブが失敗している。

- 人間のコードレビューがボトルネックになる。この実装でマージしてよいか、意図通りになっているかを確認するのが一番時間がかかる。Vercelをつかっているので一応pull request段階でpreview urlにデプロイされるところまで自動化はされているが、これを確認するのが面倒。動作確認してそれがIssueのDescription通りになっているかを確認するところも自動化できれば実用的だろうと思う。

あとで気づいたけど、CLAUDE.mdを作ってないので作ったほうがよさそう。

まだ数時間しか使ってないけど、あまり快適に使いこなせたとはいえなかった。手元に開発環境があるのであればPC上で作業したほうが快適に感じた。ブラウザ上で確認してコメントするという体験が思った以上に快適ではない。もうちょっと工夫が必要そう。

