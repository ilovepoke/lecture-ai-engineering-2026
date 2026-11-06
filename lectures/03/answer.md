# 第3回 解答

## 演習

- 第1回（素のLLM）とエージェントの回答の違い：素のLLMはモデルの知識をもとに回答するのに対し、エージェントは質問に応じて用語集や講義要約のツールを使 い、その結果を踏まえて回答する。そのため、講座固有の内容を確認して答えられる一方、ツール呼び出しの分だけLLMの呼び出し回数が増える。
- `--trace` で見えたツール呼び出し（ツール名と引数）：--- trace ---
                                                    [tool call]   make_quiz({'lecture': 2})
                                                    [tool return] make_quiz -> 問題:「量子化」とは何ですか？
                                                    答え: モデルの重みを少ないビット数（例：16ビットから4ビット）で表し、メモリ使用量や計算量を減らす技術。精度は少し下がることがある。…
                                                    [usage] RunUsage(cost=Decimal('0.0004457'), details={'text_prompt_tokens': 894}, input_text_tokens=894, input_tokens=894, output_tokens=71, requests=2, tool_calls=1)
- 1つの質問での LLM 呼び出し回数（`requests`）：2
- 追加したツールと、docstring に書いた説明：make_quiz 指定した回の用語から1つ選び、学習内容を確認する問題と答えを返す。
                                                    講義の内容を使った確認問題を求められたときに使う。lecture には回数を
                                                    整数で指定する（例: 第2回なら 2）。

## 宿題

- 取り組んだレベル：実装 
- やったこと：学習の記録を `learning_log/` に保存する `save_learning_log` ツールを作る。保存の前に利用者の確認を挟む（Pydantic AI の `requires_approval=True`）。TestModel を使ったテストも書く
- 結果：エージェントが保存ツールを呼ぶと、承認確認が表示され、learning_log/lecture-0x.mdに記録が保存された。
例）- 2026-11-06T04:44:32+00:00: 第3回を学習
- 気づき・疑問：保存される内容は実行日時と回数だけが保持される。講義内容や学習メモは保存されない。
