---
layout: post
title: DAアルゴリズムと公立高校併願制のシステムの参考実装を作った(Cline + Claude3.7 + memory-bank)
date: 2025-03-20 20:39 +0900
categories: programming
---

前回、[DA アルゴリズムと公立高校併願制について学べるサイト](https://jpn-pub-hs-multi.vercel.app/)を作りましたが、今回はそのシステムの参考実装を作りました。

[公立高校マッチングシステム](https://jpn-pub-hs-matching-ref-impl.vercel.app/)

GitHub のコードはこちら。MIT ライセンスです。

[chokkoyamada/jpn-pub-hs-matching-ref-impl: 日本の公立高校入試制度の併願制と DA アルゴリズムによるマッチングの参考実装(Cline + Claude3.7 で作成)](https://github.com/chokkoyamada/jpn-pub-hs-matching-ref-impl)

デプロイ先は前回と同じ Vercel。今回はデータベースを使う必要があったので、[Turso](https://turso.tech/)を使っています。ちょっと遅いけど無料枠なので仕方ない。全部無料枠でやる技術スタックとしては Vercel + Turso はけっこう良いと思います。

今回も Cline + Claude 3.7 で、コードは一行も書かずに、ベースをつくってもらっったあと、修正指示を繰り返して生成してもらいました。

最初のプロンプトは以下の通りです。

```
日本の公立高校は現在の制度ではほとんどが単願制をとっていますが、これを併願制(複数の希望先を申告できるようにする)とし、入学試験実施後、その成績をもとにDAアルゴリズムによって割当先の高校を決定するという制度変更が検討されています。
その併願制に対応したシステムの参考実装を作りたいと思います。

システムは、Next.js + TypeScript + TailwindCSS + Tursoで作成します。Turso以外は作成済みです。
Tursoについては以下を参考にしてください。
https://docs.turso.tech/sdk/ts/guides/nextjs

まずアーキテクチャの全体像を固めたあと、個々のタスクを洗い出し、その後、それぞれの開発に着手します。
```

今回は [Cline Memory Bank](https://docs.cline.bot/improving-your-prompting-skills/custom-instructions-library/cline-memory-bank) を使って開発プロセスやタスク管理をきちんと行っています。ただし途中から面倒になって memory-bank の更新を待たずに、単発の不具合の修正を繰り返して仕上げてしまいました。

開発時間は 5 時間くらい。基本は Auto Approve しつつ、タスクが終わるごとに変更箇所をある程度コードを確認して把握しつつ、修正指示を出していきました。
実際のコードは書いていませんが、コードを読んで具体的な修正指示はしています。
現時点の Cline + Claude 3.7 でこれくらい作れるというサンプルとして参考になるとうれしいです。
