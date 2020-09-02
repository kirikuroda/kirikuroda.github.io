---
title: 需要あるかわからないPsychoPy Coderチュートリアル
author: ''
date: '2020-09-02'
slug: psychopy-coder-demo
categories:
  - 実験
tags:
subtitle: 'いきなりやってみるタイプのCoderチュートリアル'
summary: 'いきなりやってみるタイプのCoderチュートリアル'
authors: []
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
toc: true
draft: false
---

{{%toc%}}

# はじめに

## 想定している読者層

このチュートリアルでは、以下のタイプの読者を想定しています：

- 実験プログラムを書かなきゃいけなくなった心理学（特に社会心理学）系の学部生
- 卒論で（いきなり[^1]）心理学実験のプログラムを書かなくちゃいけなくなった人
- Pythonの初心者本は一通り読んだけど、いざPsychoPyになるとよくわからない人
- Coderのどこから手を着ければいいかわからない人
- せっかくやるならBuilderじゃなくてCoderがいい、でもよくわからない、という人

- CoderにDemosってやつがあるらしいけど、英語を読むことに苦手意識がある（あるいは、Demosの存在を知らない）という人
- 時間がない人

<br>

## 既に公開されている優れたチュートリアル

PsychoPyのCoderについては、既に優れたチュートリアルがいくつか公開されています（参照：[PsychoPy Coderによる心理学実験作成チュートリアルまとめ](https://qiita.com/snishym/items/8b52db0d901cf5744463 "PsychoPy Coderによる心理学実験作成チュートリアルまとめ")）。この「チュートリアルまとめ」、およびそこで紹介されているチュートリアルのいずれにおいても、Pythonの初歩から実験実施までが丁寧に解説されています。

<br>

## このチュートリアルの特色（？）

このチュートリアルでは、Pythonの初歩の説明をすっ飛ばします。上で紹介した記事も含め、もっと優れた資料や書籍が山のようにあるからです。

その代わりに、**（社会）心理学の実験で割と使うと思われる機能**に絞って話を進めます。くわえてこのチュートリアルでは、**最初から完成したコードを見て**、それを順に解説していくという方針を取ります。「該当するコードをコピペしていけば、自分の実験も書けるんじゃないか？」と思ってもらえることを狙っています。

この試みがどこまでうまくいくかわかりませんが、とりあえずセットアップしてみましょう。

<br>

# いきなりやってみるタイプのチュートリアル

## いきなりセットアップ

- この記事では**Mac**と**PsychoPy 2020.2.3**を使ってます。**この際なので、PsychoPyをアップデートしてください。**
- [https://github.com/kirikuroda/psychopy_coder_demo](https://github.com/kirikuroda/psychopy_coder_demo "GitHubのページ")からファイルをダウンロードして、PCの適当な場所に保存してください。

<br>

## いきなり実験

- 先ほどダウンロードした`psychopy_coder_demo.py`を、PsychoPy Coder（Experiment Runner）で実行してください。

- すると、短い課題が画面に表示されます。この課題を解説していくので、まずは課題をやってみてください。1分で終わります。

- （Macで）以下のようなエラーが出る場合は、[こちらのページ（英語）](https://discourse.psychopy.org/t/keyboard-psychtoolbox-hid-failing-with-mac-os-catalina-update/9389 "Keyboard psychtoolbox/hid failing with Mac OS Catalina update")を参考にしてください。

  ```python
  File “psychtoolbox/hid.pyc”, line 137, in init
  File “psychtoolbox/hid.pyc”, line 145, in _create_queue
  FileNotFoundError: [Errno 2] No such file or directory
  ```

<br>

## 実験の構造

実験は終わりましたか？　実験の構造を確認しましょう：

1. 参加者のIDを入力
2. 教示が表示される
3. Startをクリックすると課題が始まる
4. 最初に試行数が表示される
5. 次に都市名が表示される
6. どちらの都市の人口が多いかをFかJで回答する
7. 何もせずに5秒以上経つと「Hurry up!」が表示される
8. FかJを押すと、選んだ方の都市が黄色くなり、四角で囲まれる
9. 正解不正解のフィードバックが表示される
10. （裏でその試行のデータが記録される）
11. ※4〜10を繰り返す
12. 実験終了のメッセージが表示される

<br>

では、コードを順になぞっていきます。

<br>

## 解説

### ダイアログボックスを呈示する（38–46行目）

```python
# ダイアログボックスを呈示し、参加者の情報を入力
subj_info = {"subj_id": "", "add_here_what_you_want": ""}
dialogue_box = gui.DlgFromDict(subj_info, order = ["subj_id", "add_here_what_you_want"])

# OKならID（subj_id）を記録して実験を進める。キャンセルなら実験を中止
if dialogue_box.OK:
    subj_id = subj_info["subj_id"]
else:
    core.quit()
```

参加者の情報を入力するためのダイアログボックスを呈示しています。

`add_here_what_you_want`と書いてあるように、他に記録したい情報がある場合は、後ろに付け足してください。

<br>

### データファイルの名前を作る（52–67行目）

```python
# 現在日時を記録
exp_date = data.getDateStr("%Y%m%d%H%M%S")

# データファイルを保存するフォルダを作る
# フォルダがなければ作る
try:
    os.makedirs("data/csv")
    os.makedirs("data/log")
# フォルダが既にある場合は何もしない
except OSError:
    pass

# データファイルの名前を作る（ID_日付）
file_name = subj_id + "_" + exp_date
file_name_csv = os.path.join("data/csv/" + file_name + ".csv")
file_name_log = os.path.join("data/log/" + file_name + ".log")
```

参加者のIDと現在日時からファイル名を作っています。プログラムを実行した日時は、何らかの形で記録しておくようにしましょう。

CSVファイルに行動データ、logファイルに実験のログを記録することにします。

<br>

### クイズの項目を読み込む（69–70行目）

```python
# クイズの項目（都市名、人口）を読み込む
city = pd.read_csv("city.csv")
```

この実験課題で呈示する項目（CSVファイル）を読み込んでいます。

<br>

### 画面、マウス、キーボードを設定する（76–80行目）

```python
# 画面の座標系　units = "norm"
# 画面中心が(0, 0)、X軸が-1〜+1、Y軸が-1〜+1
win = visual.Window(width = 1200, height = 900, units = "norm")
mouse = event.Mouse()
kb = keyboard.Keyboard()
```

PsychoPyでは、ふつうスクリーンを`win`として記録します。

<br>

### 都市名刺激を設定する（86–103行目）

```python
# テキスト刺激の色と日本語フォント
color_default, color_highlight = "white", "yellow"
font_ja = "ヒラギノ角ゴシック W3"

# 刺激（都市名）　textは毎試行変わるので、後で定義
city_1_text = visual.TextStim(win, font = font_ja)
city_2_text = visual.TextStim(win, font = font_ja)

# 刺激の呈示位置のカウンターバランス
# 都市名をX軸方向にどれくらいずらすか
city_nudge_x = 0.5
# 4試行のカウンターバランスをcity_posとして保存
city_pos = ["one_two", "one_two", "two_one", "two_one"]
# city_text_posを並び替える
city_pos = np.random.permutation(city_pos)

# 試行の順序をランダマイズする。trial_order: [3, 2, 0, 1]のようになる
trial_order = np.random.permutation(range(len(city)))
```

テキスト刺激は`visual.TextStim()`で作ります。日本語フォントは好きなものに修正してもらって構いません。

ここでは、呈示位置のカウンターバランスと、試行順序のランダマイズもしています[^2]。

<br>

### キーを設定する（105–112行目）

```python
# キー設定
key_left, key_right = "f", "j"

# 押すべきキーの名前（課題中に呈示しておく）
# キーのテキストをY軸方向にどれくらいずらすか
key_text_nudge_y = 0.5
key_left_text = visual.TextStim(win, text = "F", pos = (-city_nudge_x, key_text_nudge_y))
key_right_text = visual.TextStim(win, text = "J", pos = (city_nudge_x, key_text_nudge_y))
```

Fが左、Jが右に対応するようにしています。

また、課題中に呈示する「F」と「J」の刺激もここで作っています。

<br>

### 教示を定義する（114–121行目）

```python
# 教示を定義
inst_text = visual.TextStim(win, alignText = "left", anchorHoriz = "center")
inst_text.setText("""
Which city has a larger population?
Select the city by pressing the F or J of the keyboard.

Click "Start" and work on the task.
""")
```

教示を作っています。日本語だと少しめんどくさいので、英語で書いています。

`.setText()`を使うと、`TextStim`にテキストを埋め込むことができます。

<br>

### Startボタンを定義する（123–129行目）

```python
# ボックスの線の太さ
box_line_width = 10

# 開始ボタンのボックスとテキスト
start_pos_y = -0.5 # Y軸座標
start_box = visual.Rect(win, width = 0.2, height = 0.2, pos = (0, start_pos_y), lineWidth = box_line_width)
start_text = visual.TextStim(win, text = "Start", pos = (0, start_pos_y))
```

Startボタンを定義しています。四角形は`visual.Rect()`で作ります。

<br>

### その他のテキスト刺激や図形を定義する（131–157行目）

```python
# 試行間で呈示するテキスト（テキストの中身は毎試行変えるので、後で定義する）
iti_text = visual.TextStim(win)
# ITIの長さ
iti_length = 2

# どちらの都市名を選んだかを何秒呈示するか
confirmation_length = 1
# 選んだ方の都市を囲うボックスを定義
city_box_width, city_box_height = 0.5, 0.2 # ボックスのサイズ
city_box_left = visual.Rect(win,
    width = city_box_width, height = city_box_height, pos = (-city_nudge_x, 0),
    lineColor = color_highlight, lineWidth = box_line_width
)
city_box_right = visual.Rect(win,
    width = city_box_width, height = city_box_height, pos = (city_nudge_x, 0),
    lineColor = color_highlight, lineWidth = box_line_width
)

# 正解不正解のテキスト
correct_text = visual.TextStim(win, text = "Correct!")
wrong_text = visual.TextStim(win, text = "Wrong...")
feedback_length = 1

# プロンプトのテキスト
hurry_text = visual.TextStim(win, text = "Hurry up!", pos = (0, 0.8), color = color_highlight)
# time_limit秒経過したらプロンプトを出す
time_limit = 5
```

その他のテキスト刺激や図形を定義しています。

<br>

### ログファイルを設定する（163–164行目）

```python
# ログファイルの設定
file_log = logging.LogFile(file_name_log, level = logging.EXP)
```

ログファイルを設定しています。実験中に何が起きたかをほぼ自動で記録してくれるので、設定しておくことをおすすめします。

<br>

### 教示を呈示する（170–190行目）

```python
# 教示（無限ループ）
while True:

    # Startにカーソルが載ってたら黄色に
    if start_box.contains(mouse):
        start_box.setLineColor(color_highlight)
        start_text.setColor(color_highlight)
    # 載ってなければ白に
    else:
        start_box.setLineColor(color_default)
        start_text.setColor(color_default)

    # 教示とボックスを描画
    inst_text.draw()
    start_box.draw()
    start_text.draw()
    win.flip()

    # 開始ボタンがクリックされたら無限ループを抜ける
    if mouse.isPressedIn(start_box):
        break
```

教示を呈示しています。刺激を`draw()`してから`win.flip()`するのが基本です。

Startボタンにカーソルが載ったら色を変えるようにしています。これを応用すると、マウスの位置を記録することができます。

Startボタンがクリックされたらループを抜けます。

<br>

### CSVファイルの1行目に変数名を書き込む（192–199行目）

```python
# CSVファイルの先頭行に変数名を書き込む
with open(file_name_csv, "a", encoding = "cp932") as f:
    writer = csv.writer(f, lineterminator = "\n")
    writer.writerow([
        "subj_id", "trial", "city_1", "city_2",
        "population_1", "population_2", "choice",
        "correct_answer", "result", "rt", "key", "pos"
    ])
```

CSVファイルに変数名を書き込んでいます。データを記録するときは、この書き方を真似するのがおすすめです。

<br>

### カーソルを消して課題を始める（205–215行目）

```python
# カーソルを消す
mouse.setVisible(False)

# 課題開始
for trial_index in range(len(city)):

    # 試行間のテキストを定義して描画
    iti_text.setText(str(trial_index + 1) + "/" + str(len(city)))
    iti_text.draw()
    win.flip()
    core.wait(iti_length)
```

カーソルを消してから、試行のループに移っています。

`iti_text`を定義し、呈示しています。

`core.wait(秒数)`で、プログラムを指定時間止めることができます。

<br>

### 都市名を呈示（217–244行目）

```python
    # 刺激テキストをセット
    city_1 = city["city_1"][trial_order[trial_index]]
    city_2 = city["city_2"][trial_order[trial_index]]
    city_1_text.setText(city_1)
    city_2_text.setText(city_2)

    # ついでに人口と正解も記録しておく
    population_1 = city["population_1"][trial_order[trial_index]]
    population_2 = city["population_2"][trial_order[trial_index]]
    if population_1 > population_2:
        answer = "city_1"
    else:
        answer = "city_2"

    # 刺激の位置のカウンターバランス
    if city_pos[trial_index] == "one_two":
        city_1_text.setPos((-city_nudge_x, 0))
        city_2_text.setPos((city_nudge_x, 0))
    else:
        city_1_text.setPos((city_nudge_x, 0))
        city_2_text.setPos((-city_nudge_x, 0))

    # 刺激を描画
    city_1_text.draw()
    city_2_text.draw()
    key_left_text.draw()
    key_right_text.draw()
    win.flip()
```

都市名刺激を定義・描画したり、課題に関連するデータを記録したりしています。

`.setPos()`を使うと、刺激の呈示位置を変更することができます。

<br>

### 開始時間を記録し、キーボードをリセット（246–251行目）

```python
    # 回答を待ち始めた時間をresp_onsetとして記録
    resp_onset = core.Clock()

    # キー押しをリセット
    kb.getKeys([key_left, key_right], waitRelease = False)
    kb.clock.reset()
```

刺激呈示の開始時間を記録し、キーボードをリセットしています。

ここでキーボードをリセットしないと、次の試行にキー押しが引き継がれてしまいます。忘れないようにしましょう。

<br>

### 回答を待つ（253–318行目）

```python
    # 回答を待つ（無限ループ）
    while True:

        # FかJのキー押しを待つ
        key_pressed = kb.getKeys(keyList = [key_left, key_right], waitRelease = False)

        # もしFかJが押されたら
        if len(key_pressed) > 0:

            # 反応時間を記録
            rt = key_pressed[0].rt

            # どっちのキーを押したかをkeyとして記録
            # カウンターバランスに応じて、どっちの都市を選んだかをchoiceとして記録
            if key_pressed[0].name == key_left:
                key = key_left
                if city_pos[trial_index] == "one_two":
                    choice = "city_1"
                else:
                    choice = "city_2"
            else:
                key = key_right
                if city_pos[trial_index] == "one_two":
                    choice = "city_2"
                else:
                    choice = "city_1"

            # 結果を記録
            if choice == answer:
                result = "correct"
            else:
                result = "wrong"

            # 選んだ方の都市名を黄色にする
            if choice == "city_1":
                city_1_text.setColor(color_highlight)
            else:
                city_2_text.setColor(color_highlight)

            # 選んだ方の都市を囲う四角を描画
            if key == key_left:
                city_box_left.draw()
            else:
                city_box_right.draw()

            # その他の刺激も描画して、1秒間呈示
            city_1_text.draw()
            city_2_text.draw()
            key_left_text.draw()
            key_right_text.draw()
            win.flip()
            core.wait(confirmation_length)

            # 刺激の色をリセットし、無限ループから抜ける
            city_1_text.setColor(color_default)
            city_2_text.setColor(color_default)
            break

        # ※time_limitを過ぎたらプロンプトを出す
        if resp_onset.getTime() > time_limit:
            city_1_text.draw()
            city_2_text.draw()
            key_left_text.draw()
            key_right_text.draw()
            hurry_text.draw()
            win.flip()

```

回答を待ちます。反応があったらテキストを黄色にし、四角で囲みます。5秒経過したらプロンプトを出すようにしています。

コードは長めですが、ここでのキー押し判定や条件分岐は汎用性が高いです。というか、ほとんどの実験はこういう（長い）パーツの組み合わせからできています。

<br>

### フィードバックをし、各試行のデータを記録（320–338行目）

```python
    # 正解不正解のフィードバックを呈示
    if result == "correct":
        correct_text.draw()
    else:
        wrong_text.draw()
    win.flip()
    core.wait(feedback_length)

    # CSVファイルにデータを記録
    with open(file_name_csv, "a", encoding = "cp932") as f:
        writer = csv.writer(f, lineterminator = "\n")
        writer.writerow([
            subj_id, trial_index, city_1, city_2,
            population_1, population_2, choice,
            answer, result, rt, key, city_pos[trial_index]
        ])

    # ログファイルを保存
    logging.flush()
```

正解不正解のフィードバックを出してから、データを記録しています。

なお、ここでは日本語（都市名）を記録しているので、`encoding = "cp932"`としていますが、ふつうは要らないです。

<br>

### 実験終了（344–355行目）

```python
# 終わりの画面を定義
finish_text = visual.TextStim(win)
finish_text.setText("""
Finish! Thanks!
""")

# 3秒呈示してから実験終了
finish_text.draw()
win.flip()
core.wait(3)
win.close()
core.quit()
```

終了のメッセージを出して、3秒経過したら実験が終わります。

<br>

# おわりに

このチュートリアルでは割愛した内容もあります。その中で最も重要なものは「関数の自作」です。関数の自作については、[PsychoPy Coderによる心理学実験作成チュートリアルまとめ](https://qiita.com/snishym/items/8b52db0d901cf5744463 "PsychoPy Coderによる心理学実験作成チュートリアルまとめ")の第7回を読んでください。

おそらく、このチュートリアルはわかりにくいです。しかし、ここに載っている機能を使えば、大体の社会心理学実験や意思決定実験のたたき台を作ることはできると思います[^3]。かなり駆け足でしたが、このチュートリアルが誰かの役に立てば幸いです。

<br>

[^1]: 本当は「いきなり」じゃない、すなわち、ある程度想定できたことなんだけど、なかなか準備できないものです。私もそうでした。
[^2]: 試行の順序がシャッフルされているので、本来は呈示位置をシャッフルする必要はないです。一応参考のために書いています。
[^3]: そもそも、この（不親切な）説明に付いて来ようとしている時点でかなりモチベーションがあるはずです。プラスアルファで公式のリファレンスを読めば、実用に耐える実験プログラムをすぐに書けるようになると思います。