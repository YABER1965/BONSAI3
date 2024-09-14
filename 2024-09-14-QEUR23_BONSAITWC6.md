---
title: QEUR23_BONSAITWC5 - Question to Question (Step2-Task1 of 3)
date: 2024-09-05
tags: ["QEUシステム", "メトリックス", "Python言語", "LLM", "データセット", "Fine-tuning", "イノベーション","Gemma-2"]
excerpt: LLMのファインチューニングを最適化する
---

## QEUR23_BONSAITWC6 – fastHTMLで初歩的なソート表示を行う

## ～ 一言・・・。これは便利だ！！ ～

QEU:FOUNDER（設定年齢65歳）  ： “それでは、いままで生成したQ＆Aデータをデータベースに入れて、バブル・チャットに表示できるようにしましょう。”

D先生（設定年齢65歳） ： “おっと！いよいよですかぁ！！！これができれば、膨大な言語情報の把握が本当に楽になります。基本は、前回の**fastHTMLのプログラムを改造するだけ**なので、改造した一部のみを紹介します。まずは、データベースへの入力プログラムをドン！！これは、PC上で行われることを前提としています。”

```python
###########################
# テーブルを生成する
###########################
# ---
import sqlite3

dbname = 'data/todos_qa.db'
conn = sqlite3.connect(dbname)
# sqliteを操作するカーソルオブジェクトを作成
cur = conn.cursor()

# personsというtableを作成してみる
# 大文字部はSQL文。小文字でも問題ない。
cur.execute(
    'CREATE TABLE todos(id INTEGER PRIMARY KEY AUTOINCREMENT, QAtype STRING, QAcontent STRING, Goal STRING, Step STRING, Language STRING)')

# データベースへコミット。これで変更が反映される。
conn.commit()
conn.close()

```

QEU:FOUNDER ： “テーブルの設定は大きく変わりました。GOALとか、STEPの要素を追加しました。これをもとにデータベースのレコードをフィルタリングします。”

D先生： “Languageは？”

QEU:FOUNDER ： “項目の追加はしましたが、今回は適用しませんでした。もし、やってみたかったら・・・、”

D先生 ： “「自分でやってね」ということですね。それでは、つづきに行きましょう。”

```python
# ---
# csvからtableを作成する (pandas利用)
# csv内のデータをpandasデータフレームとして読み出し、データベースへ書き込む。
import sqlite3
import pandas as pd

# ---
dbname = 'data/todos_qa.db'
conn = sqlite3.connect(dbname)
# sqliteを操作するカーソルオブジェクトを作成
cur = conn.cursor()

###########################
# 連続してデータを入力する
###########################
# ---
def input_data(filename):
    # ---
    ########################################
    # READ EXCEL FILE
    ########################################
    # -----
    # EXCELファイルを読み込む
    df_excel = pd.read_excel(f'{filename}.xlsx')
    #df_excel

    ########################################
    # CONVERTING TO CONVERSATION TYPE
    ########################################
    # ----
    acc_talk = []
    for i in range(len(df_excel)):
        # ---
        str_question = df_excel.loc[i, "QInitial"]
        str_answer = df_excel.loc[i, "AInitial"]
        str_goal = df_excel.loc[i, "Goal"]
        str_step = df_excel.loc[i, "Step"]
        str_language = df_excel.loc[i, "Language"]
        # ---
        arr_question = ["question", str_question, str_goal, str_step, str_language]
        arr_goal_AB = [f"GOAL_{str_goal}_at_step{str_step}", str_answer, str_goal, str_step, str_language]
        # ---
        acc_talk.append(arr_question)
        acc_talk.append(arr_goal_AB)
    # ---
    mx_conversation = np.array(acc_talk)
    #print(mx_conversation)

    # ---
    # QAtype STRING, QAcontent STRING, done bool)
    arr_column = ["QAtype","QAcontent","Goal","Step","Language"]
    df_conversation = pd.DataFrame(mx_conversation, columns = arr_column)
    print(f"--- {filename} ---")
    print(df_conversation[0:5])

    ###########################
    # DATABASE WRITING
    ###########################
    # dbのnameをsampleとし、読み込んだcsvファイルをsqlに書き込む
    # if_existsで、もしすでにexpenseが存在していたら、書き換えるように指示
    #df_conversation.to_sql('todos', conn, if_exists='replace')
    df_conversation.to_sql('todos', conn, if_exists='append', index=False)

# ---
arr_filename = [
    "cut_TWC_BONSAI3_step2A",
    "cut_TWC_BONSAI3_step2B",
    "cut_TWC_BONSAI3_step1B",
    "cut_TWC_BONSAI3_step1A",
    "cut_TWC_BONSAI3_step2B_jp"
]

# ---
for i in range(len(arr_filename)):
    str_filename = arr_filename[i]
    #print(str_filename)
    input_data(str_filename)

# ---
# データベースへコミット。これで変更が反映される。
conn.commit()
conn.close()

```

QEU:FOUNDER ： “ここでは、データベースに情報をファイルごと入れています。”

![imageJRF3-7-1](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-1.jpg)

D先生： “あたらしく手動でデータを入れましたね？”

QEU:FOUNDER ： “そっちの方が、プログラムを作るよりも楽だからね。それでは、次に表示用のプログラムにうつりましょう。このプログラムは、Railwayのプラットフォームに「main.py」として入ることになります。。”

```python
# ----
from fasthtml.common import *

########################################
# READ DATABASE - FASTLITE
########################################
# https://answerdotai.github.io/fastlite/
db = database('data/todos_qa.db')
dbname = "todos"

global example_messages
global str_goal
global str_step

# ---
def create_message(str_goal, str_step, str_language):
    qry = f'select * from {dbname} WHERE Goal LIKE "{str_goal}%" and Step LIKE "{str_step}%" and Language LIKE "{str_language}%";'
    data = db.q(qry)
    #data[0]
    # {'index': 0,
    # 'QAtype': 'question',
    # 'QAcontent': 'Has the Japanese media accurately reported on the military tensions in East Asia since 2020, and do Chinese and Taiwanese media mirror this sentiment?',
    # 'done': 'False'}

    # ---
    example_messages = []
    for row in data:
        example_messages.append({"role": row['QAtype'], "content": row['QAcontent']})
    # ---
    #print(example_messages[0])
    
    return example_messages

# ---
# 初期絞り込み条件
str_goal = "A"
str_step = "1"
str_language = "English"
example_messages = create_message(str_goal, str_step, str_language)

##############################
# MAIN PROGRAM
##############################
# ----
app, rt = fast_app()

@app.get('/')
def homepage():

    global example_messages
    global str_goal
    global str_step

    # ----
    str_step_show = str_step
    str_goal_show = str_goal

    return Main(H1('Chat Messages'), 
    Div(cls="container")(H2(A("Link to Page 2 (to change sorting)", href="/page2")), Grid(
        P(str_step_show), P(str_goal_show))
    ),
    Div(*[create_chat_message(**msg, msg_num=i) for i, msg in enumerate(example_messages)]))

@app.get("/page2")
def page2():
    return Main(H1("Change sorting condition with the form below:"),
                Form(Select(Option('1'), Option('2'), name='step'),  Select(Option('A'), Option('B'), name='goal'), 
                     Button("Submit"),
                     action="/", method="post"))

@app.post("/")
def add_message(step:str, goal:str):

    global example_messages
    global str_goal
    global str_step

    # ---
    # 初期絞り込み条件
    str_goal = goal
    str_step = step
    str_language = "English"
    example_messages = create_message(str_goal, str_step, str_language)
    return homepage()

# ---
def create_chat_message(role, content, msg_num):
    if role == 'system': color = '#8B5CF6'
    elif role == 'question': color = "#F000B8"
    else: color = "#37CDBE"
    text_color = '#1F2937'

    # msg 0 = left, msg 1 = right, msg 2 = left, etc.
    alignment = 'flex-end' if msg_num % 2 == 1 else 'flex-start'

    message = Div(Div(
            Div(# Shows the Role
                Strong(role.capitalize()),
                style=f"color: {text_color}; font-size: 0.9em; letter-spacing: 0.05em;"),
            Div(# Shows content and applies font color to stuff other than syntax highlighting
                Style(f".marked *:not(code):not([class^='hljs']) {{ color: {text_color} !important; }}"),
                Div(content),
                style=f"margin-top: 0.5em; color: {text_color} !important;"),
            # extra styling to make things look better
            style=f"""
                margin-bottom: 1em; padding: 1em; border-radius: 24px; background-color: {color};
                max-width: 70%; position: relative; color: {text_color} !important; """),
        style=f"display: flex; justify-content: {alignment};")

    return message

serve()

```

D先生 ： “使い方を見てみましょう。まずは、初期状態から・・・。”

![imageJRF3-7-2](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-2.jpg)

QEU:FOUNDER ： “現在、初期条件としてStep2とGoal_Aが選択されています。これを変えてみましょう。**「Link to Page2」というリンク文**があるでしょ？それをクリックしてください。”

D先生： “はいよ・・・。このフォームの入力を**ドロップダウン・リスト**で変えて、と・・・。”

![imageJRF3-7-3](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-3.jpg)

QEU:FOUNDER  ： “「SUBMIT」をクリックすれば、表示される情報がかわります。”

D先生： “ほう・・・。ここではJ語で表示されていますね。”

QEU:FOUNDER ： “これは、表示後にブラウザに翻訳を指示しただけです。どうです？このプログラムは便利でしょ？”

D先生 ： “簡単、便利・・・。最高ですね。”

**(以上の内容をバブル・チャット型式で表示します。)**

[ここを、クリック！！](https://fasthtml-sorting-production.up.railway.app/)
**（注：サーバーがsleepしているかもしれないので、起動まで気長に待ってね）**

QEU:FOUNDER ： “今後は生成した情報を、随時、このfastHTMLシステムの中に入れていきましょう。”

[![MOVIE1](http://img.youtube.com/vi/_o31SB3NLFk/0.jpg)](http://www.youtube.com/watch?v=_o31SB3NLFk "How to Create & Deploy a Python Web Application FAST (fastHTML Tutorial)")

D先生 ： “この応用事例を通じて、より多くの人がfastHTMLのありがたさに気が付いてくれればうれしいですね。”



## ～ まとめ ～

### ・・・ 前回のつづきです ・・・

C部長 : “このオッサン（↓）は、いつも**「右が国をダメにする」**といっています。”

[![MOVIE2](http://img.youtube.com/vi/nYHXYBGuvt8/0.jpg)](http://www.youtube.com/watch?v=nYHXYBGuvt8 "15分朝刊チェック！：朝日新聞による維新への応援が半端ない件 2024/09/10")

QEU:FOUNDER ： “ちがうと思うよ。逆に**国が弱くなると右がでてくる」**のでしょう・・・。F国の場合、わすかに残った植民地経営の断末魔の瞬間（↓）を見たくないのでしょうね。そうだ！！今度、BONSAI3でディベートをやってみましょうか”

![imageJRF3-7-4](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-4.jpg)

QEU:FOUNDER ： “例えば・・・、**「F国の落日、右翼の跋扈、そしてGSの飛翔」**・・・。そういえば、最近、こういうニュース（↓）があって驚いた・・・。”

![imageJRF3-7-5](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-5.jpg)

C部長（設定年齢50歳前半） : “そうかな？これはフェイクじゃないのか・・・。まあ、**「V国のなりたち」**を考えてみれば、このような路線変更は必然とも考えられます。ちなみに、建国のストーリの一部は、ボクでも知っています。”

![imageJRF3-7-6](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-6.jpg)

QEU:FOUNDER ： “F国って、植民地経営がとても乱暴なのですよ。おそらく、アフリカの諸国は、**V国の独立を（我々が目指すべき）ベンチマークと見ている**はずです。”

![imageJRF3-7-7](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-7.jpg)

QEU:FOUNDER ： “さらに言えば、いつも正義面しているA国もかなりアヤシイのではないか・・・。”

![imageJRF3-7-8](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-8.jpg)

D先生： “なんで、A国がこの件に関与を！？”

QEU:FOUNDER ： “だから、**ここら辺の話は欧米全体が「同罪」じゃないのか**？今は、偉そうに自由だ、平等だと言っていながら、昔は無茶苦茶やっていたんだ。こっそりいえば「今でも」ね・・・（笑）。”

![imageJRF3-7-9](/2024-09-14-QEUR23_BONSAITWC6/imageJRF3-7-9.jpg)

C部長： “あれ？この国の旗のデザインは、V国と似ていないですか？”

QEU:FOUNDER ： “ネットで調べたところ、「たまたま似ていた」そうです。ただし、小生はあやしいとおもっていますがね・・・。”

D先生： “こうやって考えてみると、**10月は大変な月になる**。後世へのインパクトは、J国で起きること、A国で起きることなんて、とても目じゃない規模だ・・・。”

QEU:FOUNDER  ： “10月の結果次第で、世界の相、すべての前提が変わります。だから、**「10月はディエンビエンフー」**・・・。”
