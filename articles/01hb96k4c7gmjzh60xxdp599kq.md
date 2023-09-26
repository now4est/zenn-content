---
title: "macOS で Zenn のファイルを管理する際の注意点"
emoji: "👀"
type: "tech"
topics: []
published: true
---

## 環境

- macOS
    - ファイルシステムはデフォルトの設定のまま
- Zenn
    - GitHub リポジトリと連携済み

## ファイル名の大文字と小文字に注意

ファイル名には制限がある。

> 1. slugはサイト全体で（記事や本などのコンテンツの種類ごとに）ユニークにする必要があります。他ユーザーの記事で使用済みのslugも使用できないのでご注意ください。
> 2. slugは半角英小文字（a-z）、半角数字（0-9）、ハイフン（-）、アンダースコア（_）の12〜50字の組み合わせにする必要があります。
>
> via [Zennのスラッグ（slug）とは](<https://zenn.dev/zenn/articles/what-is-slug>)

ファイル名を決めるのが面倒だったので [ulid](<https://github.com/ulid/spec>) を利用した。
デフォルトでは大文字で出力されるため、Zenn 側で連携エラーが出てしまった。

そこで、ファイル名を小文字に直し、GitHub へプッシュするのだが、割りと手間取ってしまった。

まず、macOS のデフォルトのファイルシステムでは大文字と小文字が区別されないらしい。

- [あれMacって大文字と小文字を区別しないのか | DevelopersIO](<https://dev.classmethod.jp/articles/mac-apfs-ignore-case/#toc-4>)
- [Macのディスクユーティリティで利用できるファイルシステムフォーマット - Apple サポート (日本)](<https://support.apple.com/ja-jp/guide/disk-utility/dsku19ed921c/mac>)

使用途中での変更は不可のようだ。

git もデフォルトで区別しない。そこで、git config で設定を変えたのだが、ここが要注意だった。
リネームや削除を適切に git リポジトリに反映させないと、大文字のファイル名と小文字のファイル名で、ファイルが重複して登録されてしまう。
しかし、macOS のファイルシステム上では 1つのファイルしか見えない。

結局、一旦削除してからコミット・プッシュし直した。
うっかりミスをやりそうなので、git config もファイルシステムに合わせて、大文字と小文字を区別しない設定に戻しておいた。

## その他の参考サイト

割りとあるあるのようだ。

- [フォルダ/ファイル名の大文字小文字でハマった](<https://zenn.dev/soma3134/articles/20220726_folder_case_sensitive>)

- [Gitでファイル名の大文字・小文字の変更を検知するようにする - Qiita](<https://qiita.com/sawadashota/items/aa312a3b7e2403448efe>)
- [gitconfig の基本を理解する - Qiita](<https://qiita.com/shionit/items/fb4a1a30538f8d335b35>)

- [Zenn CLIで記事・本を管理する方法](<https://zenn.dev/zenn/articles/zenn-cli-guide>)
- [GitHubリポジトリでZennのコンテンツを管理する](<https://zenn.dev/zenn/articles/connect-to-github>)

- [ZMV-Examples (require autoload zmv) · GitHub](<https://gist.github.com/niksmac/77de3f19d1de0e7c20a8a0f5736c837d>)
    - `$ zmv '*' '${(L)f}'`
