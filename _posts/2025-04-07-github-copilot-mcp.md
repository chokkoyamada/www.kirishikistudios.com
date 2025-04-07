---
layout: post
title: GitHub Copilot Agent + MCP server + brave search + fetchでエラーの原因を調べてもらう
date: 2025-04-07 18:56 +0900
---

MCP Server が話題なので色々さわってみている。
まずは信頼できる MCP 実装を使って、エラーを調査できるようにしてみた。
最近 GitHub Copilot Agent で MCP が使えるようになったので、まずはそれを試してみる。
自分で実装するのも試しているがそれはまた別途書きたい。

`.vscode/settings.json` は今のところ下記のようにしている。 `mcp.json` を作る方法もあるようだ。

```json
{
  "mcp": {
    "servers": {
      "brave-search": {
        "command": "npx",
        "args": ["-y", "@modelcontextprotocol/server-brave-search"],
        "env": {
          "BRAVE_API_KEY": "BSAxxxxxxxxxxxxxxxxxxxxxxxxxxx"
        }
      },
      "fetch": {
        "command": "uvx",
        "args": ["mcp-server-fetch"]
      }
    }
  },
  "chat.agent.enabled": true
}
```

OpenAIのSDK使ってアプリを作っていたらちょうど良いケースが見つかったので、MCPを使ってエラーの原因を探させてみた。

とりあえず今回は自分でGoogle検索で解決策が見つかったことは確認している。そしてこういうエラーは、手元のソースコードを見ているだけではなかなか解決策にたどり着けない。実際、MCPを使わずにClaude 3.7 Sonnetに聞いた限りではへんてこな解決策を提示してきてしまった。

![GitHub Copilot Agent]({{site.baseurl}}/assets/images/2025-04-07-1.png)

今回の問題は、httpxのバージョンを固定していないことにより、httpxのバージョンによってエラーになってしまうということだった。

背後でhttpxを使っているということ自体知らなかった。

このように、MCPを使うと、適切に答えにたどり着けた。

現段階ではエラーメッセージを人がみてそれをコピペしてAgentに聞いて、というステップを踏んでいるが、DevinとかのAI Agentに適切に権限を付与したら、クラウド上のLambdaのエラーログを見て、そのエラーの解決策を調査してPull Requestする、というところまで自動化できたら、開発生産性にはかなり寄与すると思う。


