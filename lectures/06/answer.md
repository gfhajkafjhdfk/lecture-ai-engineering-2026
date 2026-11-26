# 第6回 解答

## 演習

- `--list-tools` で見えたツール名と、第3回（`--mcp` なし）との違い：
  - 第3回の直接登録では `search_glossary` と `get_lecture_summary` が見えた。
  - MCP経由では `glossary_search_glossary`、`glossary_get_lecture_summary`、`glossary_make_quiz` が見えた。
  - 設定済みのfetchサーバーも含めると、MCP側にはさらに `fetch_fetch` が見えた。
- `--mcp --trace` で見えたツール呼び出し（ツール名と引数）と、`requests` の回数：
  - `AIE2026の講座の宿題にある「3レベル」とは何ですか？`
  - MCPでは `glossary_search_glossary({'query': '3レベル'})` が呼ばれ、`requests=2`、`tool_calls=1` だった。
  - MCPなしでは `search_glossary({'query': '3レベル'})` が呼ばれ、同様に `requests=2`、`tool_calls=1` だった。
- MCP サーバーに公開したツールと、その説明文：
  - `glossary_search_glossary`：講座の用語集から用語を検索し、定義と扱う回を返す。
  - `glossary_get_lecture_summary`：指定した回（1始まりの整数）の講義の要点を返す。
  - `glossary_make_quiz`：指定回の用語から確認問題を1問作る。
- fetch サーバーを足したときのツール呼び出し（ツール名と引数）：
  - `fetch_fetch({'url': 'https://modelcontextprotocol.io/'})` が呼ばれ、公式サイトの内容を取得した。
  - 続けて `glossary_search_glossary({'query': 'MCP'})` も呼ばれた。`requests=3`、`tool_calls=2` だった。
  - 公式サイトの冒頭を、MCPがAIアプリと外部ツール・データをつなぐ標準規格であることなど、3点で要約した。

## 宿題

- 取り組んだレベル：実装
- やったこと：用語集から指定回の用語を選んでクイズを返す `make_quiz` を作り、MCPサーバーに `glossary_make_quiz` として登録した。
- 結果：「第99回のクイズを作って」で `glossary_make_quiz({'lecture': 99})` が呼ばれ、「第99回の用語はありません。用語がある回: 1, 2, 3, 4, 6」と返った。モデルも利用可能な回を案内した。TestModelでもMCP経由の範囲外入力をテストした。
- 気づき・疑問：MCPサーバーにツールを登録すると、エージェントのtraceに呼び出しと戻り値が表示され、モデルが戻り値を使って回答することを確認できた。
