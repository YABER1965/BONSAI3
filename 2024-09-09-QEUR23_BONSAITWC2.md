---
title: QEUR23_BONSAITWC2 : fastHTMLで初歩的なQAデータ表示を行う
date: 2024-09-09
tags: ["QEUシステム", "メトリックス", "Python言語", "LLM", "データセット", "Fine-tuning", "イノベーション","PHI-2"]
excerpt: LLMのファインチューニングを最適化する
---

## QEUR23_BONSAITWC2 : fastHTMLで初歩的なQAデータ表示を行う

## ～ 一番簡単な方法です(Railway上で) ～

QEU:FOUNDER（設定年齢65歳）  ： “今回は、BONSAI3プロジェクトの流れからいうと「閑話休題」です。ただし、今回は技術的にはfastHTMLがメインになりますので、閑話休題ではないです。「メインディッシュ」です。”

![imageJRF3-3-1](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-1.jpg)

D先生（設定年齢65歳） ： “今回は、バブル・チャットで、前回のQA出力をWeb表示するというやつですか？”

QEU:FOUNDER ： “本当は、そう行きたかったのだが、現実はトラブルにつぐトラブルです。PCスタンドアロンではfastHTMLは簡単に動くのですが、Railwayの上では全然ダメなんですよ。思いのほか、MySQLを使っても、あんまりうまく行かない。もちろん、これは自分の凡ミスの可能性は否定はできないのだが・・・。”

![imageJRF3-3-2](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-2.jpg)

D先生： “じゃあ、MySQLも使わなかったんですか？”

![imageJRF3-3-3](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-3.jpg)

QEU:FOUNDER ： “fastHTMLの中に**「fastlite」という機能**があります。これを使いましょう。まずは、sqliteのデータベースの中にデータを入れます。プログラムをドン！！”

```python
from fasthtml.common import *
import uvicorn

db = database('data/todos_qa.db')
todos = db.t.todos
if todos not in db.t: todos.create(id=int, QAtype=str, QAcontent=str, done=bool, pk='id')
Todo = todos.dataclass()

app = FastHTML(hdrs=[picolink])
rt = app.route

@patch
def __ft__(self:Todo):
   done = "✅" if self.done else ""
   link = AX(self.QAtype, hx_get=f'/QAconv/{self.id}', target_id='details')
   edit = AX('edit', hx_get=f'/edit/{self.id}', target_id='details')
   return Li(done, link, ' | ', edit, id=f'QAconv-{self.id}')

def get_newinp(): return Select(Option("Question"), Option("Goal_B"), name="QAtype", hx_swap_oob="true", id="QAtype")
def get_newinp2(): return Input(placeholder="Content", name="QAcontent", hx_swap_oob="true", id="QAcontent")
def get_footer(): return Div(hx_swap_oob='true', id="details")

# ---
@rt("/")
def get():
  todolist = Ul(*todos(), id="todolist")
  header = Form(Group(get_newinp(), get_newinp2(), Button("Add")),
                hx_post="/", target_id="todolist", hx_swap="beforeend")
  card = Card(todolist, header=header, footer=get_footer())
  contents = Main(H1("QA conversation"), card, cls="container")
  return Title("QA conversation"), contents

@rt("/")
def post(todo:Todo): return todos.insert(todo), get_newinp(), get_newinp2()

@rt("/QAconv/{id}")
def delete(id:int):
   todos.delete(id)
   return get_footer()

@rt("/QAconv/{id}")
def get(id:int):
  todo = todos[id]
  btn = Button('Delete', hx_delete=f'/QAconv/{id}', target_id=f'QAconv-{id}', hx_swap="delete")
  return P(todo.QAtype), P(todo.QAcontent), btn

@rt("/edit/{id}")
def get(id:int):
    res = Form(Group(Input(id="QAtype"), Input(id="QAcontent"), Button("Save")),
        Hidden(id="id"), CheckboxX(id="done", label='Done'),
        hx_put="/", target_id=f'QAconv-{id}', id="edit")
    return fill_form(res, todos.get(id))

@rt("/")
def put(todo: Todo): return todos.update(todo), get_footer()

if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=int(os.getenv("PORT", default=8000)))

```

QEU:FOUNDER ： “このプログラムをコマンドライン上で「python なんちゃら.py」って実行すると**localhost:8000**のwebページ上で実行画面が出てきますので、その後でシコシコとデータを入力しましょう。”

```python

from fasthtml.common import *
import uvicorn

db = database('data/todos_qa.db')
todos = db.t.todos
if todos not in db.t: todos.create(id=int, QAtype=str, QAcontent=str, done=bool, pk='id')
Todo = todos.dataclass()

app = FastHTML(hdrs=[picolink])
rt = app.route

@patch
def __ft__(self:Todo):
   done = "✅" if self.done else ""
   link = AX(self.QAtype, hx_get=f'/QAconv/{self.id}', target_id='details')
   edit = AX('edit', hx_get=f'/edit/{self.id}', target_id='details')
   return Li(done, link, ' | ', edit, id=f'QAconv-{self.id}')

def get_newinp(): return Select(Option("Question"), Option("Goal_B"), name="QAtype", hx_swap_oob="true", id="QAtype")
def get_newinp2(): return Input(placeholder="Content", name="QAcontent", hx_swap_oob="true", id="QAcontent")
def get_footer(): return Div(hx_swap_oob='true', id="details")

# ---
@rt("/")
def get():
  todolist = Ul(*todos(), id="todolist")
  header = Form(Group(get_newinp(), get_newinp2(), Button("Add")),
                hx_post="/", target_id="todolist", hx_swap="beforeend")
  card = Card(todolist, header=header, footer=get_footer())
  contents = Main(H1("QA conversation"), card, cls="container")
  return Title("QA conversation"), contents

@rt("/")
def post(todo:Todo): return todos.insert(todo), get_newinp(), get_newinp2()

@rt("/QAconv/{id}")
def delete(id:int):
   todos.delete(id)
   return get_footer()

@rt("/QAconv/{id}")
def get(id:int):
  todo = todos[id]
  btn = Button('Delete', hx_delete=f'/QAconv/{id}', target_id=f'QAconv-{id}', hx_swap="delete")
  return P(todo.QAtype), P(todo.QAcontent), btn

@rt("/edit/{id}")
def get(id:int):
    res = Form(Group(Input(id="QAtype"), Input(id="QAcontent"), Button("Save")),
        Hidden(id="id"), CheckboxX(id="done", label='Done'),
        hx_put="/", target_id=f'QAconv-{id}', id="edit")
    return fill_form(res, todos.get(id))

@rt("/")
def put(todo: Todo): return todos.update(todo), get_footer()

if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=int(os.getenv("PORT", default=8000)))

```

QEU:FOUNDER ： “こんな画面（↓）が出てきます。これは、この手の例題としてポピュラーな**「TODOリスト」**に1項目の情報を追加しただけです。”

![imageJRF3-3-4](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-4.jpg)

D先生： “なんで、これをRailway上でやらなかったんですか？”

QEU:FOUNDER ： “そうなんよ、ポイントは・・・。このSQliteは動きますよ。Railwayの上でも・・・。でも、**データベースのファイルは保存していない**と思います。またもや失敗しました。だから、いっそのことPC上でdbファイルを作って、そのファイルをRailwayに転送して動かそうとおもったんです。”

**(railway.toml)**

```python
[build]
builder = "NIXPACKS"

[deploy]
numReplicas = 1
sleepApplication = true
restartPolicyType = "ON_FAILURE"
restartPolicyMaxRetries = 10
```

**(requirements.txt)**

```python
python-fasthtml
uvicorn>=0.29
python-multipart
sqlite-utils

```

D先生： “ああ、そうか・・・。RailwayはDockerシステム上で動いているんですね。”

QEU:FOUNDER ： “ただし、簡略化されており、そのDockerfileは、あってもいいし、なくてもいいです。次は、いよいよブラウズ専用のfastHTMLプログラムの晒しです”

```python
from fasthtml.common import *
import uvicorn

db = database('todos_qa.db')
todos = db.t.todos
if todos not in db.t: todos.create(id=int, QAtype=str, QAcontent=str, done=bool, pk='id')
Todo = todos.dataclass()

app = FastHTML(hdrs=[picolink])
rt = app.route

@patch
def __ft__(self:Todo):
   str_type = P(self.QAtype)
   str_content = P(self.QAcontent)
   return Li(str_type, str_content)

def get_content(): return Div(hx_swap_oob='true', id="details")

# ---
@rt("/")
def get():
  todolist = Ul(*todos(), id="todolist")
  card = Card(todolist, header=get_content())
  contents = Main(H1("QA Conversation List"), card, cls="container")
  return Title("QA conversation"), contents

if __name__ == '__main__':
    uvicorn.run(app, host='0.0.0.0', port=int(os.getenv("PORT", default=8000)))

```

QEU:FOUNDER ： “これを実行すると、こんな感じ（↓）の閲覧画面がでてきます。”

![imageJRF3-3-5](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-5.jpg)

D先生： “私が、Appリンクを貼っておきましょう。ドン！！”

**（Railwayリンクを見てね）**

[ここをクリック！！](https://fasthtml-simple2-production.up.railway.app/)

D先生： “たしかに、データベースに情報をアップして、Web上で閲覧するのが、多量な情報を閲覧するには、よっぽど合理的ですね。それにしても、これをバブル・チャットで表示できないのは残念だ・・・。”

QEU:FOUNDER  ： “まあ、その件は、あとで挑戦してみますよ。どっちみち、これから情報が増えてくるのでシステム改造が必要なんです。”


## ～ まとめ ～

QEU:FOUNDER ： “ああ・・・。かわいそう・・・。”

[![MOVIE1](http://img.youtube.com/vi/4G7RIdNheYA/0.jpg)](http://www.youtube.com/watch?v=4G7RIdNheYA "斎藤知事 「嘘八百」は嘘だった！【横田一×西谷文和 とざいトーザイ】 20240903")

C部長 : “あれ？**「かわいそう」**と聞こえました。空耳かな・・・？”

![imageJRF3-3-6](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-6.jpg)

QEU:FOUNDER ： “そうだよ。小生は、「（彼が）かわいそうだ」と思っています。その理由は、**年齢（↑）**を見れば、わかるじゃないですか・・・。”

![imageJRF3-3-7](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-7.jpg)

C部長 : “そうねえ・・・、彼は**新自由主義の洗脳世代**か・・・。彼の青春時代にはメディアが全力で、礼賛していました。あの頃は、ホントにひどかった・・・。あの頃の洗脳がパワハラ癖という副作用を引き起こしたわけですね。”

![imageJRF3-3-8](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-8.jpg)

QEU:FOUNDER ： “ちょっと話を一般化しよう。経営者のマインドから言えば、ビジネスプロセスにおいて、インプットとアウトプットが一元（比例関係）にあるのが望ましい。しかし、プロセスのコンピューター化や社会・製品の高度化でプロセスの一元化が難しくなり始めた。それで、**追い詰められた経営者側がやること**は？”

C部長 : “そのための楽ちんなソルーションとしては、**従業員をパワハラで追い詰める**でしょう。つまり、**「なにがなんでも（プロセスを）一元化させる」**。”

[![MOVIE2](http://img.youtube.com/vi/d90y-49f-gw/0.jpg)](http://www.youtube.com/watch?v=d90y-49f-gw "東大教授ガチ講義！線形思考と回帰的思考の違い。原因があるから結果がある、の発想はあなたの人生をおかしくする。安冨歩東大教授。")

QEU:FOUNDER ： “ブラック企業の問題の本質って、**「プロセス特性の一元化」**なんだよね。それが彼らの「身についている」んですよ。小生が驚いたのは、まさか**お役所で、そんなことが起こっている**とはおもっていませんでした。”

C部長 : “本来は、お役所は結果よりもプロセスを重視すべき組織なんですがねえ・・・。”

![imageJRF3-3-9](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-9.jpg)

QEU:FOUNDER ： “いろいろな意味で、**あの頃はそういう時代**だったと思うしかないんです。さらに、今回の件で特に不快に思うのが、急に叩き始めた人が出て来たからなんです。”

![imageJRF3-3-10](/2024-09-09-QEUR23_BONSAITWC2/imageJRF3-3-10.jpg)

QEU:FOUNDER ： “風向きだけを見ている人たちが多すぎて、うんざりしない？”

[![MOVIE3](http://img.youtube.com/vi/QO5KB29rhNo/0.jpg)](http://www.youtube.com/watch?v=QO5KB29rhNo "日本弱体化の根本原因はここにある「弱きを叩き強きに媚びる」")

C部長 : “ようするに、**「以前は強かった人が弱くなったから、ホイホイとよろこんで叩きに来た」**だけでしょ？立派な**J国人の姿**ではないですか。”
