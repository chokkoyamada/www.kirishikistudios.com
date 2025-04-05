---
layout: post
title: DevinのCoreプランを使い始めた。個人プロジェクトでPull Requestを2つ送ってもらってマージするところまで
date: 2025-04-05 16:25 +0900
---

Devin が 2.0 になり、Core プランという新しいプランができて初期費用 20 ドルから使えるようになったので、早速個人開発で使ってみた。

アカウント登録して$20 チャージすると早速使い始められる。
まず、既存の GitHub レポジトリに接続したり、Slack チャンネルに招待したり(しなくてもいい)、Devin 用の VirtualMachine をセットアップしたりといった前準備をする。

![devin setup]({{site.baseurl}}/assets/images/2025-04-04-1.png)

開発途中の[chokkoyamada/AvailableX](https://github.com/chokkoyamada/availableX/)というレポジトリを Devin と接続して、タスクをお願いしてみた。

(1)適当にタスクを探してもらってダミーの Pull Request をしてもらう

![devin PR 1]({{site.baseurl}}/assets/images/2025-04-05-2.png)

Pull Request はこちら。

[Fix React Hooks dependencies warnings by devin-ai-integration[bot] · Pull Request #1 · chokkoyamada/AvailableX](https://github.com/chokkoyamada/AvailableX/pull/1)

(2)自動テストを実装してもらう

![devin PR 2]({{site.baseurl}}/assets/images/2025-04-05-3.png)

Pull Request はこちら。

[Add testing setup with Jest and React Testing Library by devin-ai-integration[bot] · Pull Request #2 · chokkoyamada/AvailableX](https://github.com/chokkoyamada/AvailableX/pull/2)

手元で動作確認をしてみたら `npm run test` が通らなかったので指摘したら修正してくれた。
そもそもDevinのVirtual Machineでテストを実行するセットアップもあり、今回はそれをスキップしてしまっていたのでテストが通らないPull Requestがあがってきてしまったが、それを有効にしていればテストが通る前提でタスクをやってくれるようだ。

このPull Requestをマージしたあと、Cline使ってGitHub ActionsにCIするWorkflowを追加した。
ClineとDevinを使い分けするのも特に問題なくできる。

普通に作業できていて、とても丁寧な仕事ぶりで好感が持てる。
使い始める手順もスムーズで、特に詰まるところは無かった。

会話も自然で、必要に応じて追加質問もしてくれるし、チームメンバーとしてぜひ入ってほしいと感じる。業務でも使いたくなった。

Cline はどちらかというと AI がドライバー/人間がナビゲータとなってペアプロをしている感じだが、Devin はパートの業務委託の人にタスクをお願いする感覚に近い。

ソフトウェア開発をする AI Agent がここまでの質で仕事をしてくれるというのは、新しい時代を感じる。
