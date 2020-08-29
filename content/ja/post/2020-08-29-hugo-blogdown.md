---
title: Hugoとblogdownを使って最速でウェブサイトを公開する
author: ''
date: '2020-08-29'
slug: make-website-with-hugo-blogdown
categories:
  - 執筆
tags:
subtitle: '最速？'
summary: '最速？'
authors: []
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
toc: true
draft: true
---

# はじめに

[先日の投稿](https://kirikuroda.github.io/post/2020/08/21/hugo-academic/ "「Hugo Academicでウェブサイトをリニューアルしました」")に「サイト構築の詳しい手順は説明しません」と書きましたが、気が変わったので、必要最低限の手順を残しておくことにしました。

<br>

# このページで説明すること・しないこと

表題のとおり、このページではHugoとblogdownを使って「**最速でウェブサイトを公開する**」ことを目指します。そのため、作ったウェブサイトには必要最低限の情報、すなわち、

- 自己紹介
- 論文情報／業績
- 連絡先／SNSのリンク

しか載せません。それ以外の情報や手順、たとえば、

- ブログ投稿
- 多言語対応
- CSS（見た目）のカスタマイズ
- デモページの削除　etc.

については説明しません。このページを参考にしてウェブサイトを作ろうとするくらいモチベーションがあるならば、そこまでの説明が要らないような気もするからです[^1]。

また、それぞれの手順が何を意味しているのか（例：なぜGitHubでリポジトリを作らなきゃいけないか）についても、詳しくは説明しません。これも最速のためです。

それでは説明スタートです。

<br>

# 1. GitHubでリポジトリを作る

**GitHubでリポジトリを作ってください。**初めてリポジトリを作るという方は、[『GitHubアカウント作成とリポジトリの作成手順』](https://qiita.com/kooohei/items/361da3c9dbb6e0c7946b "GitHubアカウント作成とリポジトリの作成手順")を読んでください。

設定画面での注意点は以下の2つです：

1. **リポジトリ作成時の設定はすべてデフォルトのまま**にしてください。
2. **リポジトリ名は「（GitHubの）ユーザ名.github.io」**がおすすめです。この場合、できあがったウェブサイトのURLは「https://ユーザ名.github.io」になります。そうしなかった場合、ウェブサイトのURLは「https://ユーザ名.github.io/リポジトリ名」になります。

<br>

 # 2. リポジトリ設定の修正

リポジトリを作成したら、`Settings`をクリックしてください。

下の方にある`GitHub Pages`という項目を見つけたら、`None`を`Master`に、`/(root)`を`/docs`に変更してください。

その後、右にある`Save`をクリックしたら、修正完了です。

<br>

# 3. リポジトリをクローンする

**上で作ったリポジトリをクローン[^2]します。**初めての方は、[『リポジトリをクローンする』](https://docs.github.com/ja/github/creating-cloning-and-archiving-repositories/cloning-a-repository "リポジトリをクローンする")を読んでください。

<br>

# 4. RとRStudioをインストールする

**RとRStudioをインストールします。**初めての方は、[『R と R Studioのインストール』](https://qiita.com/hujuu/items/ddd66ae8e6f3f989f2c0 "Rと R Studioのインストール")を読んでください。インストール済みの方は、スキップしてください。

<br>

# 5. Rでblogdownパッケージをインストールする

RStudioを立ち上げ、以下のコードをコンソールで実行します。

```R
install.packages("blogdown") # blogdownのインストール
```

<br>

インストールが終わったら、以下の手順でRプロジェクトを作ります。

1. `File > New Project > New Directory > Website using blogdown`をクリック
2. **上で作ったGitHubのリポジトリ名**を`Directory name`に入力
3. **クローンしたリポジトリの1個上のディレクトリ**を`Create project as subdirectory of`で指定
4. `Hugo theme`で好きなテーマを指定。ここでは`gcushen/hugo-academic`とします
5. その他はデフォルトのまま
6. `Create Project`をクリック

<br>

# 6. config.tomlを修正する

Rプロジェクトができたら、ルートディレクトリにある`config.toml`を開き、

- title
- baseurl
- hasCJKLanguage

を以下のように修正してください。

```toml
title = "Hugo Academic" # サイト名（なんでもいいです）
baseurl = "https://github.io/GitHubのユーザ名" # リポジトリ名を「ユーザ名.github.io」にした場合
# baseurl = "https://github.io/GitHubのユーザ名/リポジトリ名"　※リポジトリ名が「ユーザ名.github.io」じゃない場合
hasCJKLanguage = true
```

<br>

次に、

- publishDir
- relativeulrs
- LanguageCode

を追加します。以下のように書き足してください（どこでもいいですが、Advanced options belowの下辺りに書いておきましょう）

```toml
publishDir = "docs"
relativeurls = true
languageCode = "ja-jp"
```

<br>

# 7. params.tomlを修正する

次に、ルートディレクトリにある`params.toml`を開き、以下の内容を修正します：

- **連絡先情報**：`Contact details`の下に、メールアドレスなどの項目があるので、適宜編集してください。表示したくない項目については、空欄にしてください。
- 一番下に`ai = false`という箇所があるので、それを`ai = true`にしておく。



[^1]:そもそも、色々検索するくらいモチベーションがある人にとって、このページの説明自体どれだけ必要なのか、需要があるのかという気もします。
[^2]: リポジトリを自分のコンピュータにコピーし、同期させること。