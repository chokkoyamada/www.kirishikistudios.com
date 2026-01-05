---
layout: post
title: "DuckDBでALBのログを集計するClaude Code Skillを作った"
date: 2026-01-05 19:30:00 +0900
---

以前書いた [S3 にある ALB ログの調査は Athena より DuckDB のほうが簡単 - road288 の日記](https://road288.hatenablog.com/entry/2024/11/06/113954) の記事の続きで、これを Claude Code の Agent Skill として公開した。

[https://github.com/chokkoyamada/duckdb-alb-log-analyzer](https://github.com/chokkoyamada/duckdb-alb-log-analyzer)

![Claude Code Skill]({{site.baseurl}}/assets/images/2026-01-05-01.png)

README に書いてあるが、Claude Code の Plugin 形式になっているのでプラグインとしてインストールできる。

```
/plugin marketplace add chokkoyamada/duckdb-alb-log-analyzer
/plugin install duckdb-alb-log-analyzer
```

DuckDB CLI のインストールや AWS の環境変数のロードを助けてくれるようにしている(環境変数から使うのか profile から使うのか等をガイドしてくれる)。

前回ブログ記事を書いたときから ALB のログのカラムが増えたようで、そのままでは動かなくなってしまったのでテーブルのスキーマを更新した。

最新の ALB ログのフォーマットは公式ドキュメントの以下にあってそれを参考にした。

[Access logs for your Application Load Balancer - ELB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-access-logs.html)

DuckDB のテーブル定義としては以下になる。

[https://github.com/chokkoyamada/duckdb-alb-log-analyzer/blob/main/scripts/load_template.sql](https://github.com/chokkoyamada/duckdb-alb-log-analyzer/blob/main/scripts/load_template.sql)

{% raw %}

```
CREATE OR REPLACE TABLE {{TABLE_NAME}} AS
SELECT *
FROM read_csv(
    '{{S3_PATH}}',
    columns={
        'type': 'VARCHAR',
        'timestamp': 'TIMESTAMP',
        'elb': 'VARCHAR',
        'client_ip_port': 'VARCHAR',
        'target_ip_port': 'VARCHAR',
        'request_processing_time': 'DOUBLE',
        'target_processing_time': 'DOUBLE',
        'response_processing_time': 'DOUBLE',
        'elb_status_code': 'INTEGER',
        'target_status_code': 'VARCHAR',
        'received_bytes': 'BIGINT',
        'sent_bytes': 'BIGINT',
        'request': 'VARCHAR',
        'user_agent': 'VARCHAR',
        'ssl_cipher': 'VARCHAR',
        'ssl_protocol': 'VARCHAR',
        'target_group_arn': 'VARCHAR',
        'trace_id': 'VARCHAR',
        'domain_name': 'VARCHAR',
        'chosen_cert_arn': 'VARCHAR',
        'matched_rule_priority': 'VARCHAR',
        'request_creation_time': 'TIMESTAMP',
        'actions_executed': 'VARCHAR',
        'redirect_url': 'VARCHAR',
        'error_reason': 'VARCHAR',
        'target_port_list': 'VARCHAR',
        'target_status_code_list': 'VARCHAR',
        'classification': 'VARCHAR',
        'classification_reason': 'VARCHAR',
        'conn_trace_id': 'VARCHAR',
        'transformed_host': 'VARCHAR',
        'transformed_uri': 'VARCHAR',
        'request_transform_status': 'VARCHAR'
    },
    delim=' ',
    quote='"',
    escape='"',
    header=false,
    auto_detect=false,
    nullstr='-'
);
```

{% endraw %}

この Skill 自体は [Claude Code 公式の skill creator](https://github.com/anthropics/skills/blob/main/skills/skill-creator/SKILL.md) を使って作ったのでほぼ vibe coding。

自分では実際にこの Skill を便利に使っている。
