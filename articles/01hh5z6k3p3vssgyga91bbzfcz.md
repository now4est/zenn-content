---
title: "Vim でよく使う Vim じゃない機能"
emoji: "🎄"
type: "tech"
topics: [Vim, zsh]
published: true
---

こんにちは。[42 Tokyo](<https://42tokyo.jp/>) というパリ発のエンジニア養成機関の [2023 年度アドベントカレンダー](<https://qiita.com/advent-calendar/2023/42tokyo>) 8日目を担当します、在校生の now4est と申します。
([言い訳](<https://ja.wikipedia.org/wiki/30%E6%99%82%E9%96%93%E5%88%B6>)が通用しないタイムスタンプになってしまいました 🙇)

7日目の昨日は、snara さんが「[コマンドラインで作曲したい！〜 MML to MIDI compiler のようなものを作ってみた〜 #Go - Qiita](<https://qiita.com/snara-42/items/547a5a63d711a290d33a>)」という記事を書かれていました。
Qiita デイリーいいね数ランクイン記事です。要チェックですね。

## 今日の記事は

よくわからないタイトルですみません。主に外部コマンドとの連携のお話です。
自分がよく使っているものを紹介します。`-clipboard` `-terminal` な Vim でも使えるはずです。
よりスマートな方法も色々とあると思います。ぜひ教えてください。
また、間違い等がございましたら、こちらもぜひお知らせください。

## ファイル全体をクリップボードへコピーする

MacOS 向け

```
:w !pbcopy
```

GNU/Linux なら

```
:w !xsel -ib
```

など

あるいは、厳密には「コピー」ではなく「切り取り」相当の挙動になりますが

```
:%!pbcopy
```

```
:%!xsel -ib
```

実行後に `u` キーで undo すると良い感じです。

## 現在の行をクリップボードへコピーする

```
:.!pbcopy
```

`%` や `.` で範囲を指定しています。詳しくは Vim の `:help range` で調べてみてください。Web ブラウザで確認するなら [help - Vim日本語ドキュメント](<https://vim-jp.org/vimdoc-ja/>)で。
`!` の後にシェルコマンドやシェルスクリプトを続けます。シェルコマンドには標準入力として渡され、実行されます。

本来はテキストを外部で加工して Vim へ戻す機能です。
「切り取り」相当ではなく、より「コピー」っぽくするなら (zsh 環境が前提ですが)

```
:.!tee >(pbcopy)
```

ただし、これも編集履歴に残ったりと若干癖があります。何より面倒なので使わないです。
`tee` コマンドは別の形でよく使います。

## 管理者権限で保存

これです。今までの組み合わせです。

```
:w !sudo tee >/dev/null %
```

シェル側で保存を行っているので、Vim 終了時に未保存のメッセージが表示されます。Don't Panic
`%` は `:help cmdline-special` で確認できます。
ネット上に詳しい解説記事がたくさんありますので、詳細はググってみてください。

## 編集中のファイルのパスをコピーする

これも `%` の活用です。(zsh 前提)

```
:!pbcopy <<< '%:p'
```

`<<<` は zsh の here-string です。詳しくは `man zshall` で検索してみてください。

## シェルコマンドの実行結果を読み込む

write ときたら read です。

```
:r !ls -1
```

`ls` コマンドの結果などは、42 の課題でも便利ですね。

## シェルのコマンド履歴を読み込む

作業ログをまとめる際に便利です。(zsh 前提)

```
:r !fc -R ~/.zsh_history && fc -ln
```

`~/.zsh_history` は適宜置き換えてください。`$HISTFILE` で確認できると思います。
`fc` は `history` の実態です。インタラクティブシェル向けの機能を別の場所で利用するには若干工夫が必要です (たぶん)
履歴ファイルを読み込ませ出力させたものを Vim で読む感じです。`fc` については `man zshall` で。

## まとめ

Vim を使いこなすのは大変ですが、外部との連携さえできれば、それらをシームレスに有効活用できます。
今日は書ききれませんでしたが、オススメは jq や pandoc などです。うまくやり取りできれば、ターミナルとの行き来が無くなるので、いわゆる「Vim でやった方が早い」率がさらに高まります 😁

## 明日は

corvvs さんが「暗号学的ハッシュ関数」について書いてくれる予定です、そちらの記事もお楽しみに！
