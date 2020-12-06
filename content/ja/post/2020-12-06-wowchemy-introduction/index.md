---
title: Wowchemy（旧Hugo Academic）でウェブページをとりあえず作る
author: ''
date: '2020-12−06'
slug: hugo-academic
categories:
  - 執筆
tags:
subtitle: 'Wowchemy入門'
summary: 'Wowchemy入門'
authors: []
featured: no
image:
  placement: 1
  focal_point: "Center"
  preview_only: false
projects: []
toc: true
draft: true
---

{{%toc%}}

# はじめに

2020年秋頃に、旧Hugo AcademicがWowchemyとしてリリースされました。それに伴い、このサイトもアップデートしました。見た目は全く変わっていないですが、実は色々と改善されています。

このポストでは、Wowchemyで「とりあえずウェブページを公開する」方法を説明します。

GitHubで公開することを想定

1. remotes::install_github('rstudio/blogdown') blogdownをインストール
2. new_site(theme = "wowchemy/starter-academic")
3. うまくいくと、ローカルサーバでウェブサイトが立ち上がる。失敗した場合は、.RprofileでHugoのバージョンをしていするoptions(blogdown.hugo.version = "0.79.0")
4. Wowchemy ver. 5.0.0で動作を確認

<br>

# このポストで説明すること・しないこと

このポストではWowchemyを使って「**とりあえずウェブページを公開する**」ことを目指します。そのため、作ったウェブサイトには必要最低限の情報、すなわち、

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

下の方にある`GitHub Pages`という項目を見つけたら、`None`を`Main`に、`/(root)`を`/docs`に変更してください。

その後、右にある`Save`をクリックしたら、修正完了です。

<br>

# 3. リポジトリをクローンする

**上で作ったリポジトリをクローン[^2]してください。**初めての方は、[『リポジトリをクローンする』](https://docs.github.com/ja/github/creating-cloning-and-archiving-repositories/cloning-a-repository "リポジトリをクローンする")を読んでください。

<br>

# 4. RとRStudioをインストールする

**RとRStudioをインストールしてください**（インストール済みの方は、スキップしてOKです）。初めての方は、[『R と R Studioのインストール』](https://qiita.com/hujuu/items/ddd66ae8e6f3f989f2c0 "Rと R Studioのインストール")を読んでください。

<br>

# 5. Rでblogdownパッケージをインストールする

RStudioを立ち上げ、以下のコードをコンソールで実行してください。

```R
install.packages("blogdown") # blogdownのインストール
```

<br>

インストールが終わったら、以下の手順でRプロジェクトを作ってください。

1. `File > New Project > New Directory > Website using blogdown`をクリック
2. **上で作ったGitHubのリポジトリ名**を`Directory name`に入力
3. **クローンしたリポジトリの1個上のディレクトリ**を`Create project as subdirectory of`で指定
4. `Hugo theme`で好きなテーマを指定。ここでは`wowchemy/starter-academic`とします
5. その他はデフォルトのまま
6. `Create Project`をクリック
7. しばらくすると、デモページが開きます

<br>

# 6. config.tomlを修正する

Rプロジェクトができたら、ルートディレクトリにある`config.toml`を開き、

- title
- baseurl
- hasCJKLanguage

を以下のように修正してください。

```toml
title = "Hugo Academic" # サイト名（なんでもいいです。自分の名前とかが良いと思います）
baseurl = "https://GitHubのユーザ名.github.io/" # リポジトリ名を「ユーザ名.github.io」にした場合
# baseurl = "https://github.io/GitHubのユーザ名/リポジトリ名"　※リポジトリ名が「ユーザ名.github.io」じゃない場合
hasCJKLanguage = true
```

<br>

次に、

- publishDir
- relativeulrs
- LanguageCode

を以下のように書き足してください。

ファイルのどこに書いても良いと思いますが、Advanced options belowの下辺りに書いておきましょう。

```toml
publishDir = "docs"
relativeurls = true
languageCode = "ja-jp"
```

<br>

# 7. ページや設定を修正する

これはググってくれや

# 8. ページを公開する

git pushなどを書く

[^1]: そもそも、色々検索するくらいモチベーションがある人にとって、このページの説明自体どれだけ必要なのか、どれだけ需要があるのかという気もします。
[^2]: リポジトリを自分のコンピュータにコピーし、同期させること。