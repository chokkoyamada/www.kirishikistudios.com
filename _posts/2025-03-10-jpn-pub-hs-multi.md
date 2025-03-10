---
layout: post
title: DAアルゴリズムと公立高校併願制について学べるサイトをCline + Claude 3.7で作った
date: 2025-03-10 19:40:49 +0900
categories: programming
---

Twitter(X)で以下がバズっているのをみて、とても良いと思ったのだけど、実際どういうものかわかりやすく視覚的に学べるサイトがあればいいなと思って作ってみました。

X ユーザーの河野太郎さん: 「現在、ほとんどの都道府県では、公立高校の入試に単願制、つまり一人の生徒が一つの公立高校にしか出願できない制度をとっています。 単願制には、公平性の観点から大きな問題があります。」 / X https://x.com/konotarogomame/status/1896854394362364323

専門家のレビューを経たものではないので、修正点・改善点いただければ修正いたします。というかツッコミいただけると嬉しいです(Pull Request でも良いです)。

いま流行りの Cline + Claude 3.7 で、コードは一行も書かずに、修正指示を何往復か繰り返して生成してもらいました。
かかった時間は 1 時間ちょっと。もう手で書くより断然速くて恐ろしい。

DA アルゴリズムと公立高校併願制について学べるサイト https://jpn-pub-hs-multi.vercel.app/

GitHub のコードはこちら。

[chokkoyamada/jpn-pub-hs-multi: DA アルゴリズムと公立高校併願制のデモ](https://github.com/chokkoyamada/jpn-pub-hs-multi)
