---
layout: post
title: Ruby 3.4でCircularArgumentReferenceエラーがなくなった
date: 2025-06-13 19:34 +0900
---

Ruby 2.7~3.3 までは[CircularArgumentReference](https://www.rubydoc.info/gems/rubocop/RuboCop/Cop/Lint/CircularArgumentReference)エラーで SyntaxError になっていたコードが、Ruby 3.4 からは許容されるようになった。

## Ruby 3.3 までの挙動

以下のようなコードは、Ruby 3.3 までは SyntaxError になり、そもそも実行できなかった。

```ruby
➜  ~ docker run --rm -it ruby:3.3 irb

irb(main):001* def bake(pie: pie)
irb(main):002*   pie.heat_up
irb(main):003> end
<internal:kernel>:187:in `loop': (irb):1: circular argument reference - pie (SyntaxError)
	from /usr/local/lib/ruby/gems/3.3.0/gems/irb-1.13.1/exe/irb:9:in `<top (required)>'
	from /usr/local/bin/irb:25:in `load'
	from /usr/local/bin/irb:25:in `<main>'
irb(main):004> bake
(irb):4:in `<main>': undefined local variable or method `bake' for main (NameError)

bake
^^^^
	from <internal:kernel>:187:in `loop'
	from /usr/local/lib/ruby/gems/3.3.0/gems/irb-1.13.1/exe/irb:9:in `<top (required)>'
	from /usr/local/bin/irb:25:in `load'
	from /usr/local/bin/irb:25:in `<main>'
```

これは引数の初期値で自分自身を参照することによる循環参照として検出されていた。

## Ruby 3.4 での変更

Ruby 3.4 からは、このような循環参照が許容されるようになった：

```ruby
➜  ~ docker run --rm -it ruby:3.4 irb

irb(main):001* def bake(pie: pie)
irb(main):002*   pie.heat_up
irb(main):003> end
irb(main):004>
=> :bake
```

キーワード引数の初期値で自分自身を参照した場合、その時点では変数が未定義なので nil が代入される。
ちょっとわかりづらいから、別にこれは許容しなくても良かったんじゃないかと思って経緯を ChatGPT の Deep Research に調べてもらった。

https://chatgpt.com/share/684c0350-7684-8007-8073-b0456983a799

> 最終的に Matz の判断で「多少奇妙なコードであっても言語が強制的に阻止しない」という方針が示され
> この変更の意図は、Ruby の設計理念である**「驚き最小の原則」**にも照らし合わせて捉えることができます。奇異なコードに対して言語仕様で過剰に介入するより、一貫したルールでシンプルに解釈し、必要なら開発者側で検知・対処すれば良いという考え方

良し悪しはともかく、納得感はある。スッキリした。
