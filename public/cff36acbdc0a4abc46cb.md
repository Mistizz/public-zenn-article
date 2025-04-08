---
title: MCPサーバーのTool名のハイフン"-"とアンダースコア"_"でハマった話
tags:
  - JavaScript
  - MCP
  - cursor
private: false
updated_at: '2025-04-08T14:10:13+09:00'
id: cff36acbdc0a4abc46cb
organization_url_name: null
slide: false
ignorePublish: false
---

## はじめに

最近、界隈で話題になっている MCP（Modular Code Plugin）サーバーに興味を持って実装を始めたのですが、Cursorから動かそうとしたときにはまったことを共有します。

# 結論

なんてことはないです。
tool名にはハイフン`-`を使わず、アンダースコア`_`を使いましょう、っていう知ってる人には至極当たり前の話…たぶん


# 問題

自作のMCPサーバーで`count-chars`というtoolを実装しました。

![count-chars](https://github.com/Mistizz/public-zenn-article/blob/main/images/mcptoolname_04.png?raw=true)

それをCursorから呼び出してもらうのですが、何度やってもハイフンがアンダースコアになった`count_chars`を呼んでしまい、`Tool count_chars not found`というエラーが出て呼び出しに失敗する、という事象に悩まされました。

![実行](https://github.com/Mistizz/public-zenn-article/blob/main/images/mcptoolname_01.png?raw=true)

![エラー](https://github.com/Mistizz/public-zenn-article/blob/main/images/mcptoolname_02.png?raw=true)

Cursor Setting の MCP を見てみると、Toolsに、`count_chars`と表示されています。

![tool読み込み](https://github.com/Mistizz/public-zenn-article/blob/main/images/mcptoolname_03.png?raw=true)


# Tool名の実装を、ハイフン`-`からアンダースコア`_`に変えたら解決した

結局、Tool名の実装をハイフン`-`からアンダースコア`_`に変えることで、呼び出し時のTool名と実装してるTool名を一致させたら、エラーなく実行できるようになりました。

色々調べてみたところ、そもそもJavascriptには、変数名でハイフン`-`が使えないという制限があるようです。（地味に知らなかった…）可能性として、Typescript SDKか、もしくはCursor内部の処理系で、`-` が `_` に自動変換されているのではないか、という結論に至っています。

（Javascriptのライブラリでも、内部的にハイフン`-`をアンダースコア`_`に変換するようになってるものが多いらしい…知らんかった…）

# そうして出来上がったMCPサーバー：Japanese Text Analyzer

日本語テキストの形態素解析を行えるMCPサーバーです。
文字数をカウントしたり、品詞の割合を計算したりできます。
例えば、「小学校低学年でもわかりやすい文章」って指示して生成された文章が本当に指示通りになっているかをチェックしたいときに活躍すると思います。

https://github.com/Mistizz/mcp-JapaneseTextAnalyzer

# まとめ

MCPのTool名は、アンダースコア`-`で！
同じようにハマっている人の一助になれば幸いです。




