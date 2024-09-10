---
title: QEUR23_BONSAITWC4 – fastHTMLで初歩的なバブルチャット表示を行う
date: 2024-09-10
tags: ["QEUシステム", "メトリックス", "Python言語", "LLM", "データセット", "Fine-tuning", "イノベーション","PHI-2"]
excerpt: LLMのファインチューニングを最適化する
---

## QEUR23_BONSAITWC4 – fastHTMLで初歩的なバブルチャット表示を行う

## ～ これでブラウザの基本構造ができたと思います ～

QEU:FOUNDER（設定年齢65歳）  ： “前回で、初回（STEP1）のFBAの生成が終わりました。これをもとにブラウザ表示用のアプリを生成しました。今回は、このアプリを改造して、**「バブル・チャット」方式**に変更します。”

![imageJRF3-5-1](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-1.jpg)

D先生（設定年齢65歳） ： “ディベート生成というBONSAI3のシナリオとは外れています。それでも、回答テキスト表示のレベルを上げることはとても重要です。”

QEU:FOUNDER ： “今回も前回のシンプル版と同じ構成です。最初はユーザーのPC上でSQLiteデータベースを生成し、表示用のプログラム類とデータベースをRailwayにアップロードします。最初は、データベースの生成プログラムをドン！！”

```python
# ----
import sqlite3
import pandas as pd
import numpy as np

###########################
# CREATE DATABASE
###########################
import sqlite3

# todos_qa.dbを作成する
# すでに存在していれば、それにアスセスする。
dbname = './data/todos_qa.db'
conn = sqlite3.connect(dbname)

# データベースへのコネクションを閉じる。(必須)
conn.close()

###########################
# テーブルを生成する
###########################
#import sqlite3

dbname = 'data/todos_qa.db'
conn = sqlite3.connect(dbname)
# sqliteを操作するカーソルオブジェクトを作成
cur = conn.cursor()

# personsというtableを作成してみる
# 大文字部はSQL文。小文字でも問題ない。
cur.execute(
    'CREATE TABLE todos(id INTEGER PRIMARY KEY AUTOINCREMENT, QAtype STRING, QAcontent STRING, done bool)')

########################################
# READ EXCEL FILE
########################################
# -----
# EXCELファイルを読み込む
df_excel = pd.read_excel('qq_TWC_BONSAI3_step1B(3).xlsx')
df_excel

```

QEU:FOUNDER ： “データベースの「箱(db)」を作成して、中にテーブルを定義しました。その後で、今回入れるデータを、外部から呼び込みます。”

![imageJRF3-5-2](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-2.jpg)

D先生： “今回抽出するデータは、「QInitial」と**「FBA」**というコラムにある情報ですね。”

QEU:FOUNDER ： “このデータをJSONのデータ型式に変換しましょう。”

```python
########################################
# CONVERTING TO CONVERSATION TYPE
########################################
# ----
acc_talk = []
for i in range(len(df_excel)):
    # ---
    str_question = df_excel.loc[i, "QInitial"]
    str_answer = df_excel.loc[i, "FBA"]
    arr_question = ["question", str_question, 'False']
    arr_goal_A = ["GOAL_B", str_answer, 'False']
    # ---
    acc_talk.append(arr_question)
    acc_talk.append(arr_goal_A)
# ---
mx_conversation = np.array(acc_talk)
#print(mx_conversation)

# ---
# QAtype STRING, QAcontent STRING, done bool)
arr_column = ["QAtype","QAcontent","done"]
df_conversation = pd.DataFrame(mx_conversation, columns = arr_column)
df_conversation

```

QEU:FOUNDER ： “ここまでで、EXCELファイルのデータがバブルチャットで出力可能になります。”

![imageJRF3-5-3](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-3.jpg)

D先生： “ここの「done」って、今回は使うんですか？”

QEU:FOUNDER  ： “そうだそうだ・・・。（doneを）消すのを忘れていたわい。Todo_listの遺物なので、もういらないですよ。つづきましょう・・・。”

```python
###########################
# DATABASE WRITING
###########################
# dbのnameをtodos_qaとし、読み込んだcsvファイルをsqlに書き込む
df_conversation.to_sql('todos_qa', conn, if_exists='replace')

# データベースへコミット。これで変更が反映される。
conn.commit()
conn.close()

```

D先生： “すいません。ここで、入力した中身を見ることができませんか？”

```python
###########################
# DATABASE WRITING TO DF
###########################
# データベースからデータフレームを生成する
import sqlite3
import pandas as pd

dbname = 'data/todos_qa.db'
conn = sqlite3.connect(dbname)
# sqliteを操作するカーソルオブジェクトを作成
cur = conn.cursor()

# dbをpandasで読み出す。
df_sql = pd.read_sql('SELECT * FROM todos', conn)
#df_sql

# ---
cur.close()
conn.close()

```

QEU:FOUNDER ： “それでは、データフレーム型式で見てみましょう。”

![imageJRF3-5-4](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-4.jpg)

D先生： “きちんと、データベースの中にチャット情報が入っていますね。次は、この情報を読み込んでチャット表示をします。”

QEU:FOUNDER ： “このプログラムはpandasが入っていないので、Railwayでも行けるはずです。ドン！！”

```python
# ----
from fasthtml.common import *

########################################
# READ DATABASE - FASTLITE
########################################
# https://answerdotai.github.io/fastlite/
db = database('todos_qa.db')
dbname = "todos"
qry = f"select * from {dbname};"
data = db.q(qry)
#data[0]
# {'index': 0,
# 'QAtype': 'question',
# 'QAcontent': 'Has the Japanese media accurately reported on the military tensions in East Asia since 2020, and do Chinese and Taiwanese media mirror this sentiment?',
# 'done': 'False'}

# ---
example_messages = []
for row in data:
    #print(f"--- NO: {row['index']} ---")
    #print(row['QAtype'])
    #print(row['QAcontent'])
    # ---
    example_messages.append({"role": row['QAtype'], "content": row['QAcontent']})
# ---
#print(example_messages[0])

##############################
# MAIN PROGRAM
##############################
# ----
app, rt = fast_app()

@app.get('/')
def homepage():
    return Div(*[create_chat_message(**msg, msg_num=i) for i, msg in enumer-ate(example_messages)])

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

D先生： “PC上での前処理があったとはいえ、ずいぶん短いコードになりました。”

![imageJRF3-5-5](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-5.jpg)

QEU:FOUNDER ： “もちろん、こんなに簡単でもRailway上でシステムはちゃんと動きます。しかも、これはブラウザ上で動いているので、テキストが表示された後で翻訳も簡単です。”

**（Step1, FBA, GROUP_Bの情報をまとめました）**

[クリックしてね・・・。](https://fasthtml-simple3-production.up.railway.app)

QEU:FOUNDER ： “ちなみに、前回のURLアドレスと同じです。”


## ～ まとめ ～

C部長 : “あれ？AIがいきなり、**「この地方（↓）」の組織**のポストを紹介してきました。もう、J国は、あそこを国として認定したんでしたっけ？”

![imageJRF3-5-6](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-6.jpg)

QEU:FOUNDER ： “まだ承認していないと思いますよ？すでに、多くの国が国家認定しているのに、(J国は)おかしいですね。”

![imageJRF3-5-7](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-7.jpg)

QEU:FOUNDER ： “G7なんかという、**「型遅れのバス」に乗っている**ことがそもそもの間違いなんです。たしか、スペインはすでに承認しているでしょ？・・・こういう、細かい部分をグローバル・サウス(GS)に見られているんだと思うよ。”

C部長 : “ちょっと実感がわきません。GSって、すごいんですか？”

![imageJRF3-5-8](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-8.jpg)

QEU:FOUNDER ： “G7って、お金（USD）をたくさん持っているんでしょう。・・・でも、そのお金で希少資源を買えなければどうしようもないです。つまり、**De-dollarizationって、クリティカルです**。”

C部長 : “１０月のBRICS会議って、どうなるんでしょうね？”

![imageJRF3-5-9](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-9.jpg)

QEU:FOUNDER ： “もうね、なるようにしかならないんじゃない？ちなみに、小生はJ国の政治にはほとんど興味はないし、年末のA国のトップ交代にも興味がない。”

![imageJRF3-5-10](/2024-09-10-QEUR23_BONSAITWC4/imageJRF3-5-10.jpg)

QEU:FOUNDER ： “むしろ、F国の今後に、すごく興味がある。GSをもっとも力で抑えつけているのは、F国だから・・・。残念ながら、**Ｆ国にはＧＳを抑えつけるだけの国力は残されているのか？**”

C部長 : “お～ぉ・・・、こわっ・・・。”

QEU:FOUNDER ： “F国の状況が変わると、全てが変わるんですよ。”
