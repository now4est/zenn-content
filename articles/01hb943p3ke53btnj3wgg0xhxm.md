---
title: "Google データ エクスポートに若干の仕様変更?"
emoji: "👀"
type: "tech"
topics: []
published: true
---

大量に開いた、iPhone の Chrome のタブを一括エクスポートするために「Google データ エクスポート」を活用している。
ここ数年は安定していたが、ダウンロードしたデータ内に `BrowserHistory.json` が無く、スクリプトがエラー停止していた。

---

確認してみる。

<https://takeout.google.com/?hl=ja&utm_source=google-account&utm_medium=web>

- 「Chrome」にチェック
- 「Chrome のすべてのデータが含まれます」をクリック
    - この時点で、以前はあった BrowserHistory が無い
    - History が同等のようだ

エクスポートを実行し、ダウンロード。
ディレクトリ構造等に変更はなし。単純に `BrowserHistory` が `History` へ変更されただけのようだ。
