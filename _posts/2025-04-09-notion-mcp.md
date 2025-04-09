---
layout: post
title: Notion公式のMCPを使って検索とコメントを試した
date: 2025-04-09 07:55 +0900
---

Notion公式の MCP Serverがリリースされていたので試してみた。
[makenotion/notion-mcp-server: Official Notion MCP Server](https://github.com/makenotion/notion-mcp-server)

MCP Serverに使うNotionのintegration tokenは以下から生成できる。
ただし、管理者権限が必要なようだ。自分が管理権限のないWorkspaceではIntegrationを有効化できなかった。

[My Creator Profile | Notion](https://www.notion.so/profile/integrations)

生成したinternal integration Secretを以下のJSONに含めて自分が浸かっているMCP Clientの設定に追加する。私はClineを使っている。なおGitHub Copilot AgentのMCPではなぜか「Failed to validate tool 2d9_API-post-database-query: TypeError: Cannot use 'in' operator to search for 'type' in true」というエラーになって使えなかった。

```
{
  "mcpServers": {
    "notionApi": {
      "command": "npx",
      "args": ["-y", "@notionhq/notion-mcp-server"],
      "env": {
        "OPENAPI_MCP_HEADERS": "{\"Authorization\": \"Bearer ntn_****\", \"Notion-Version\": \"2022-06-28\" }"
      }
    }
  }
}
```

Notionは以前使っていたけどしばらく管理者としてさわっていなかったため、知識が無くてハマったのだが、Notion APIで検索にヒットさせるようにするには、ページ単位でConnectしておく必要がある。

Notion公式の下記のページにも書いてある。

[インテグレーションの追加と管理 – Notion (ノーション)ヘルプセンター](https://www.notion.com/ja/help/add-and-manage-connections-with-the-api#%E3%83%9A%E3%83%BC%E3%82%B8%E3%81%B8%E3%81%AE%E3%82%B3%E3%83%8D%E3%82%AF%E3%83%88%E3%81%AE%E8%BF%BD%E5%8A%A0)

下記の画像のように、ページの三点リーダーの「Connections」から、先程作ったIntegrationを追加しておく必要がある。親ページだけやれば良く、その配下の子ページには設定が伝播するようだ。

![Notion Connect]({{site.baseurl}}/assets/images/2025-04-09-1.png)

これでMCP Clientとやりとりする準備は整った。まずやりたいのはNotionの情報を引っ張ってきてコンテキストに含めることだ。
ページのリストの取得もできるし、検索もできる。

![Notion Connect 2]({{site.baseurl}}/assets/images/2025-04-09-2.png)
![Notion Connect 3]({{site.baseurl}}/assets/images/2025-04-09-3.png)
![Notion Connect 4]({{site.baseurl}}/assets/images/2025-04-09-4.png)

コメントの追加もできた。

![Notion Connect 5]({{site.baseurl}}/assets/images/2025-04-09-5.png)

できた事自体は「おぉ〜！」という驚きと喜びはあるけど、この1ステップのIntegrationだったらただのAPI連携だし、手動でやっても大した手間ではない。ここから、もうちょっと大きな粒度のタスクや自動化フローをまるっと任せることができてこそ意味があると思う。

そうなると、ClineとかClaude Desktopのような対話型インタフェースではなく、横で待機していて必要なときにフォローしてくれるようなAmbientなAgentがいいなあと思う。



