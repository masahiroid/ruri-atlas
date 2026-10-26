---
title: Ruri Atlas
emoji: 🗺️
colorFrom: blue
colorTo: indigo
sdk: static
pinned: false
models:
  - cl-nagoya/ruri-v3-30m
  - cl-nagoya/ruri-v3-130m
  - hotchpotch/japanese-reranker-xsmall-v2
---

# Ruri Atlas — see the meaning of Japanese & English text / 日本語と英語の意味を、ブラウザで見る

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.23123353.svg)](https://doi.org/10.5281/zenodo.23123353)

[English](#english) | [日本語](#日本語)

**Live demo:** https://huggingface.co/spaces/masahiroid/ruri-atlas  ·  Source: this repository (static site: `index.html`, `extra.js`, `en.json`)


## English

**In short:** type a phrase or a situation in Japanese *or English* and watch a 3-D map of sentences light up. Sentences with similar meaning sit close together; the closest ones glow, and an optional reranker reorders them. It runs entirely in your browser (WebGPU) — nothing you type is sent anywhere. Pick the map language (日本語 / English / JA+EN) at the top right, or tap one of the example chips at the bottom to try it.

A zero-install demo of **Japanese semantic search that runs entirely in your browser (WebGPU, with a WASM fallback)**. 96 short Japanese sentences are embedded
with [ruri-v3-30m](https://huggingface.co/cl-nagoya/ruri-v3-30m) and drawn as a glowing map (PCA plus a light relaxation). Type a query: it lands on the map, the nearest
sentences light up, and a Japanese cross-encoder reranker ([japanese-reranker-xsmall-v2](https://huggingface.co/hotchpotch/japanese-reranker-xsmall-v2), ONNX export:
[masahiroid/japanese-reranker-xsmall-v2-onnx-web](https://huggingface.co/masahiroid/japanese-reranker-xsmall-v2-onnx-web)) reorders them. Nothing you type leaves the page: the models are
downloaded once and run on your GPU through [transformers.js](https://github.com/huggingface/transformers.js).

- **Try different embedding models** from the selector (ruri-v3-30m / 130m, multilingual-e5-small, paraphrase-multilingual-MiniLM): the same 96 sentences re-arrange themselves and the same query can return different neighbours, which makes the differences in Japanese handling visible. Works on phones (drag, pinch, tap).
- **Bilingual (v0.6)**: switch the map between 日本語 / English / JA+EN (UI language follows). English sentences are machine translations of the Japanese ones (Amazon Nova Pro), so every sentence has a translation pair; in JA+EN mode hover a point to see its pair and check whether a multilingual model really places them together. Rerankers: Japanese → japanese-reranker-xsmall-v2, English → ms-marco-MiniLM-L-6-v2, JA+EN → bge-reranker-base (multilingual, ~0.5 GB). English embedding models: bge-small-en-v1.5, all-MiniLM-L6-v2. All ONNX exports are existing community/Xenova ones; nothing was re-published.
- **Prompt-injection viewer** (🛡 button): pick one of 8 Japanese injection patterns, place it at the start / middle / end of a document, and see (a) which documents light up red because they are semantically close to the instruction, (b) how far the host document drifts once the instruction is mixed in, and (c) how its retrieval rank changes. This visualizes closeness in embedding space only — whether an LLM actually obeys the instruction is measured separately with [japanese-llm-security](https://github.com/masahiroid/japanese-llm-security) / model-audit-lite.
- The map now holds 368 short Japanese sentences (8 topics).
- **Drag to rotate the 3-D map, scroll to zoom**; it orbits slowly when idle, and after a search the camera glides to face the query. Click a dot to use its sentence as the query. Toggle the reranker to see how the order changes.
- Why it matters for RAG: embedding similarity gets candidates quickly; the reranker is slower but judges whether a sentence actually answers the query. The panel shows both scores.
- The map is a 3-D projection (PCA, then a light relaxation) of 256-dimensional vectors; distances on the map are only indicative.
- Related: the [Core ML / TFLite / MLX conversions](https://github.com/masahiroid/japanese-llm-security) and the Swift / Kotlin RAG libraries by the same author.

**Models used** — embedding (selectable): [sirasagi62/ruri-v3-30m-ONNX](https://huggingface.co/sirasagi62/ruri-v3-30m-ONNX) (a community ONNX export of [cl-nagoya/ruri-v3-30m](https://huggingface.co/cl-nagoya/ruri-v3-30m), Apache-2.0), [ruri-v3-130m-ONNX](https://huggingface.co/sirasagi62/ruri-v3-130m-ONNX), [Xenova/multilingual-e5-small](https://huggingface.co/Xenova/multilingual-e5-small) (from intfloat/multilingual-e5-small), [Xenova/paraphrase-multilingual-MiniLM-L12-v2](https://huggingface.co/Xenova/paraphrase-multilingual-MiniLM-L12-v2);
reranker: [masahiroid/japanese-reranker-xsmall-v2-onnx-web](https://huggingface.co/masahiroid/japanese-reranker-xsmall-v2-onnx-web) (our ONNX export of [hotchpotch/japanese-reranker-xsmall-v2](https://huggingface.co/hotchpotch/japanese-reranker-xsmall-v2), MIT; a larger [small-v2](https://huggingface.co/masahiroid/japanese-reranker-small-v2-onnx-web) is also available).

### License and citation

Apache-2.0 (see [LICENSE](LICENSE)). Please cite via [`CITATION.cff`](CITATION.cff). The English sentences in `en.json` are machine translations of the Japanese sentences (Amazon Nova Pro). Models are loaded in the browser from their own Hugging Face repositories under their own licenses.

## 日本語

**ひとことで:** 言葉や状況を、日本語でも**英語でも**入力すると、文章の3Dの地図の上で意味の近い文が光ります。意味が近い文は近くに並び、近いものほど明るく光り、リランカーで並べ替えもできます。すべてブラウザの中（WebGPU）で動き、入力した内容はどこにも送られません。右上で地図の言語（日本語 / English / 日＋英）を選べ、下の例文ボタンを押すとすぐ試せます。

インストール不要で、**日本語のセマンティック検索をブラウザの中だけ（WebGPU。非対応ならWASM）で動かす**デモです。短い日本語の文96件を
[ruri-v3-30m](https://huggingface.co/cl-nagoya/ruri-v3-30m) で埋め込み、光る地図として描きます（PCAと軽い整列）。言葉を入力すると、地図上に現れ、近い文章が光り、
日本語のクロスエンコーダ（[japanese-reranker-xsmall-v2](https://huggingface.co/hotchpotch/japanese-reranker-xsmall-v2) のONNX版:
[masahiroid/japanese-reranker-xsmall-v2-onnx-web](https://huggingface.co/masahiroid/japanese-reranker-xsmall-v2-onnx-web)）が並べ替えます。入力した文章はページの外に出ません。
モデルは初回にダウンロードされ、[transformers.js](https://github.com/huggingface/transformers.js) であなたのGPUで動きます。

- **埋め込みモデルを切り替えて比べられます**（ruri-v3-30m / 130m、multilingual-e5-small、paraphrase-multilingual-MiniLM）。同じ96文が並び替わり、同じ検索語でも近くに出る文章が変わるので、日本語の扱いの違いが見えます。スマホ（ドラッグ・ピンチ・タップ）でも使えます。
- **日英バイリンガル（v0.6）**: 地図を 日本語 / English / 日＋英 で切り替えられます（画面の言語も連動）。英語の文は日本語の文の機械翻訳（Amazon Nova Pro）なので、すべての文に翻訳ペアがあります。日＋英モードでは点にマウスを載せると翻訳ペアが線で結ばれ、多言語モデルが本当に近くに置いているか確かめられます。リランカーは 日本語→japanese-reranker-xsmall-v2、英語→ms-marco-MiniLM-L-6-v2、日＋英→bge-reranker-base（多言語・約0.5GB）。英語の埋め込みは bge-small-en-v1.5 / all-MiniLM-L6-v2。ONNX はすべて既存の公開物を使い、再公開はしていません。
- **インジェクション表示**（🛡ボタン）: 日本語の指示攻撃8パターンから選び、文書の先頭・途中・末尾に混ぜると、(a) その指示と意味が近い文書が赤く光る、(b) 混ぜた文書が意味の上でどれだけ動くか、(c) 検索順位がどう変わるか、が見えます。見ているのは埋め込み空間での近さだけで、LLMが実際に従うかどうかは [japanese-llm-security](https://github.com/masahiroid/japanese-llm-security) / model-audit-lite のプローブで別に測ります。
- 地図の例文は368文（8トピック）に増えました。
- **ドラッグで3Dの地図を回し、スクロールで拡大・縮小**できます（放っておくとゆっくり回り、検索するとカメラが検索語の方へ向きます）。点をクリックすると、その文章が検索語になります。リランカーのオン・オフで、順位の変化を見られます。
- RAGで大事な理由: 埋め込みの類似度は候補を素早く集め、リランカーは遅い代わりに「その文が本当に答えになっているか」を判断します。右のパネルに両方のスコアを出しています。
- 地図は256次元のベクトルの3次元への投影（PCAと軽い整列）で、距離はあくまで目安です。
- 関連: 同じ作者の [Core ML / TFLite / MLX 変換](https://github.com/masahiroid/japanese-llm-security) と、Swift / Kotlin のRAGライブラリ。

**使用モデル** — 埋め込み（切り替え可）: [sirasagi62/ruri-v3-30m-ONNX](https://huggingface.co/sirasagi62/ruri-v3-30m-ONNX)（[cl-nagoya/ruri-v3-30m](https://huggingface.co/cl-nagoya/ruri-v3-30m) のコミュニティ版ONNX、Apache-2.0）、[ruri-v3-130m-ONNX](https://huggingface.co/sirasagi62/ruri-v3-130m-ONNX)、[Xenova/multilingual-e5-small](https://huggingface.co/Xenova/multilingual-e5-small)（intfloat/multilingual-e5-small 由来）、[Xenova/paraphrase-multilingual-MiniLM-L12-v2](https://huggingface.co/Xenova/paraphrase-multilingual-MiniLM-L12-v2)。
再ランキング: [masahiroid/japanese-reranker-xsmall-v2-onnx-web](https://huggingface.co/masahiroid/japanese-reranker-xsmall-v2-onnx-web)（[hotchpotch/japanese-reranker-xsmall-v2](https://huggingface.co/hotchpotch/japanese-reranker-xsmall-v2) の自作ONNX、MIT。より大きい [small-v2](https://huggingface.co/masahiroid/japanese-reranker-small-v2-onnx-web) もあります）。

### ライセンスと引用

Apache-2.0（[LICENSE](LICENSE) を参照）。引用は [`CITATION.cff`](CITATION.cff) をご利用ください。`en.json` の英語の文は、日本語の文の機械翻訳です（Amazon Nova Pro）。モデルは、それぞれの Hugging Face リポジトリから、それぞれのライセンスのもとでブラウザに読み込まれます。
