---
title: QEUR23_BS3TWC0: BONSAI3 – ADVANCED_DEBATE(その2)
date: 2024-09-02
tags: ["QEUシステム", "メトリックス", "Python言語", "LLM", "データセット", "Fine-tuning", "イノベーション","PHI-2"]
excerpt: LLMのファインチューニングを最適化する
---

## QEUR23_BS3TWC0: BONSAI3 – ADVANCED_DEBATE(その2)

## ～ 実は、このテーマ（秘密）をやりたかったんだ ～


QEU:FOUNDER（設定年齢65歳） ： “じゃあ、BONSAI3のプロジェクトを改めて始めましょう。今回はテーマを変えてね・・・。”

![imageJRF3-1-1](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-1.jpg)

D先生（設定年齢65歳） ： “ROUND2-3もずいぶん長くなりましたね。今回も、新しいテーマでディベートをやるんでしょう？”

![imageJRF3-1-2](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-2.jpg)

QEU:FOUNDER ： “**BONSAIシステムにインプットするテーマは新しくなりました。**こんな感じ（↓）で・・・・。”

![imageJRF3-1-3](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-3.jpg)

C部長： “EXCELファイルのキャプチャだけじゃ、細かな内容がわからないです。何を議論するんですか？”

![imageJRF3-1-4](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-4.jpg)

QEU:FOUNDER ： “今回は特別です。内容は終盤になるまで公開しません。今回のテーマがLLMで正常に出力するのか、かなり「あやしい」んですよ。もちろん、メディアでは頻繁に議論されています。C部長に見せるよ。この件・・・（ヒソヒソ・・・）。”

C部長： “あぁ・・・。この件なのか・・・。アライメントがきついLLMでは正常に回答がでてこないかもしれないですね。すくなくとも、相当に都度内容の改変があるでしょうね。そういえば、今回においては**EVALUATORのキャラがかわりました**ね。かなりイケてますね（笑）？”

![imageJRF3-1-5](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-5.jpg)

QEU:FOUNDER ： “じつはね・・・。最近、小生は思いっきり**V国推し**なんです（笑）。実は、今回のEvaluatorはこの人（↑）をイメージしています。おっとっと・・・。話を変えましょう。今回の目玉は、この件じゃないです。今回から、新しいテクノロジとして**「fastHTML+Railway」**を導入します。便利だよ、fastHTMLって・・・。”

![imageJRF3-1-6](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-6.jpg)

QEU:FOUNDER ： “一見、普通のホームページをfastHTMLで組みなおすと以下のようになります。”

```python
# ---
from fasthtml.common import *

# ---
page = Html(
            Head(
                Meta(charset='UTF-8'),
                Meta(name='viewport', content='width=device-width, initial-scale=1'),
                Title('LPテンプレート'),
                Meta(name='keywords', content=''),
                Meta(name='description', content=''),
                Link(rel='icon', type='image/png', href='img/favicon.png'),
                Link(rel='stylesheet', media='all', href='css/ress.min.css')(
                    Link(rel='stylesheet', media='all', href='css/style.css'),
                    Script(src='js/jquery-3.6.0.min.js'),
                    Script(src='js/style.js'),
                    Link(rel='icon', type='image/png', href='img/favicon.png')
                )
            ),
            Body(
                Header(
                    Div(cls='container')(
                        Div(cls='row')(
                            Div(cls='col span-12 header')(
                                H1(
                                    A('LOGO', href='')
                                ),
                                Div(cls='header-box')(
                                    A(href='')(
                                        Span('お問い合わせ', cls='contact-button')
                                    )
                                )
                            )
                        ),
                        Div(cls='row')(
                            Div(cls='col span-12')(
                                Nav(
                                    Div(id='open'),
                                    Div(id='close'),
                                    Div(id='navi')(
                                        Ul(
                                            Li(
                                                A('ホーム', href='index.html')
                                            ),
                                            Li(
                                                A('商品について', href='#1')
                                            ),
                                            Li(
                                                A('お客様の声', href='#2')
                                            ),
                                            Li(
                                                A('申し込みの流れ', href='#3')
                                            ),
                                            Li(
                                                A('アクセス', href='#4')
                                            ),
                                            Li(
                                                A('お問い合わせ', href='#5')
                                            )
                                        )
                                    )
                                )
                            )
                        )
                    )
                ),
                Div(cls='mainimg')(
                    Img(src='img/mainimg.jpg', alt='メイン画像')
                ),
                Main(
                    Div(cls='catch')(
                        H2(
                            Span('キャッチフレーズ', cls='under')
                        ),
                        P(
                            'ここにキャッチコピーが入ります。',
                            Br(),
                            'お店の宣伝やコンセプトなど。魅せる文言を入力。'
                        )
                    ),
                    Section(id='1', cls='gray-back')(
                        H2(cls='center')(
                            Span('商品について', cls='under')
                        ),
                        Div(cls='container center')(
                            Div(cls='row')(
                                Div(cls='col span-4')(
                                    Img(src='img/product.jpg', alt='ここに商品'),
                                    H3('ここに商品が入ります'),
                                    P('ここに説明文が入ります')
                                ),
                                Div(cls='col span-4')(
                                    Img(src='img/product.jpg', alt='ここに商品'),
                                    H3('ここに商品が入ります'),
                                    P('ここに説明文が入ります')
                                ),
                                Div(cls='col span-4')(
                                    Img(src='img/product.jpg', alt='ここに商品'),
                                    H3('ここに商品が入ります'),
                                    P('ここに説明文が入ります')
                                )
                            )
                        )
                    ),
                    Section(id='2')(
                        H2(cls='center')(
                            Span('スタッフ紹介', cls='under')
                        ),
                        Div(cls='container center')(
                            Div(cls='row')(
                                Div(cls='col span-4')(
                                    Img(src='img/staff.jpg', alt='スタッフ'),
                                    H3('スタッフ'),
                                    P('ここに説明文が入ります')
                                ),
                                Div(cls='col span-4')(
                                    Img(src='img/staff.jpg', alt='スタッフ'),
                                    H3('スタッフ'),
                                    P('ここに説明文が入ります')
                                ),
                                Div(cls='col span-4')(
                                    Img(src='img/staff.jpg', alt='スタッフ'),
                                    H3('スタッフ'),
                                    P('ここに説明文が入ります')
                                )
                            )
                        )
                    ),
                    Section(id='3', cls='gray-back')(
                        H2(cls='center')(
                            Span('申し込みの流れ', cls='under')
                        ),
                        Div(cls='container')(
                            Div(cls='row flow')(
                                Div(cls='col span-3')(
                                    Img(src='img/flow.jpg', alt='申し込み')
                                ),
                                Div(cls='col span-9')(
                                    P('ここに文章が入ります。ここに文章が入ります。ここに文章が入ります。')
                                )
                            ),
                            Div(cls='row flow')(
                                Div(cls='col span-3')(
                                    Img(src='img/flow.jpg', alt='申し込み')
                                ),
                                Div(cls='col span-9')(
                                    P('ここに文章が入ります。ここに文章が入ります。ここに文章が入ります。')
                                )
                            ),
                            Div(cls='row flow')(
                                Div(cls='col span-3')(
                                    Img(src='img/flow.jpg', alt='申し込み')
                                ),
                                Div(cls='col span-9')(
                                    P('ここに文章が入ります。ここに文章が入ります。ここに文章が入ります。')
                                )
                            )
                        )
                    ),
                    Section(id='4')(
                        H2(cls='center')(
                            Span('アクセス', cls='under')
                        ),
                        Div(cls='container')(
                            Div(cls='row')(
                                Div(cls='col span-12')(
                                    Iframe(src='https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d27814443.96425483!2d120.28897720705172!3d31.679877148840735!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x34674e0fd77f192f%3A0xf54275d47c665244!2z5pel5pys!5e0!3m2!1sja!2sjp!4v1555153587836!5m2!1sja!2sjp', width='100%', height='450', frameborder='0', style='border:0', allowfullscreen='')
                                )
                            )
                        )
                    ),
                    Section(id='5')(
                        H2(cls='center')(
                            Span('お問い合わせ', cls='under')
                        ),
                        Div(cls='container')(
                            Div(cls='row')(
                                Div(cls='col span-6')(
                                    Div(cls='contact-box')(
                                        P(
                                            Img(src='img/tel.png', alt='電話')
                                        ),
                                        P('012-345-6789')
                                    )
                                ),
                                Div(cls='col span-6')(
                                    Div(cls='contact-box')(
                                        P(
                                            Img(src='img/mail.png', alt='Eメール')
                                        ),
                                        P('simple@mail.com')
                                    )
                                )
                            ),
                            Div(cls='row')(
                                Div(cls='col span-12')(
                                    Form(method='post', action='')(
                                        Table(cls='table full-width')(
                                            Tbody(
                                                Tr(
                                                    Th(
                                                        Label('お名前', fr='name')
                                                    ),
                                                    Td(
                                                        Input(type='text', name='お名前', placeholder='名前を記入', id='exampNameInput', cls='full-width')
                                                    )
                                                ),
                                                Tr(
                                                    Th(
                                                        Label('メールアドレス', fr='email')
                                                    ),
                                                    Td(
                                                        Input(type='email', name='Email', placeholder='メールアドレスを記入', id='exampleEmailInput', cls='full-width')
                                                    )
                                                ),
                                                Tr(
                                                    Th(
                                                        Label('お電話番号', fr='tel')
                                                    ),
                                                    Td(
                                                        Input(type='tel', name='お電話番号', placeholder='お電話番号を記入', id='exampleTellInput', cls='full-width')
                                                    )
                                                ),
                                                Tr(
                                                    Th(
                                                        Label('お問い合わせ内容', fr='exampleMessage')
                                                    ),
                                                    Td(
                                                        Textarea(name='お問い合わせ内容', placeholder='用件をご記入ください …', id='exampleMessage', cls='full-width textarea')
                                                    )
                                                )
                                            )
                                        ),
                                        P(cls='center')(
                                            Input(type='submit', value='送 信', cls='button')
                                        )
                                    )
                                )
                            )
                        )
                    )
                ),
                Footer(
                    Div(cls='container')(
                        Div(cls='row')(
                            Div(cls='col span-4')(
                                H4('フッター1'),
                                P('ここにSNSやテキストなどが入ります。SNSやテキストなどが入ります。')
                            ),
                            Div(cls='col span-4')(
                                H4('フッター2'),
                                P('ここにSNSやテキストなどが入ります。SNSやテキストなどが入ります。')
                            ),
                            Div(cls='col span-4')(
                                H4('フッター3'),
                                P('ここにSNSやテキストなどが入ります。SNSやテキストなどが入ります。')
                            )
                        )
                    )
                ),
                Div(cls='copyright')(
                    A('Copyright © QEU.', href='https://jpnqeur23bonsai2.blogspot.com/2024/08/qeur23chrltm46-step3-task2-of-4-fact.html', target='_blank')
                ),
                P(id='pagetop')(
                    A('TOP', href='#')
                )
            )
        )

# ---
#print(to_xml(page))

# ---
app,rt = fast_app()

@rt('/')
def get(): return page

serve()

```

D先生 ： “モロにPythonのプログラムですね。”

C部長： “あれ？**Railway**は・・・？”

![imageJRF3-1-7](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-7.jpg)

QEU:FOUNDER ： “「アプリ(app)」をrailwayサーバーにアップすれば、誰でも使えるようになるプラットフォームです。ちょっと使ってみてください。リンク（↓）を貼っているので・・・。”

[Railwayアプリを見るには、ココをクリック](fasthtml-deploy-production.up.railway.app)

D先生 ： “う～ん・・・。Herokuという先駆者があったとはいえ、便利な時代になったものだな・・・。さっきの、きれいなホームページをRailwayに載せることはできますか？”

QEU:FOUNDER ： “それがねえ・・・。PC上では難なく動いても、Railway上では、なぜかいつもエラーが出るんです。小生が、Railwayについて、現状であまり語りたくないのも、そのためです。使ってみたければ、自分で調べてください。本当に便利ですよ。”

D先生 ： “そもそも、今回のディベートで、なぜRailwayをつかおうと思ったんですか？”

![imageJRF3-1-8](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-8.jpg)

QEU:FOUNDER ： “ディベートって、情報量がとても多いじゃないですか。いつも、我々は、その一部分だけを抽出しています。**「fastHTML+Railway」で、アプリを通して全部を表示できるようになるんです。**”

C部長： “それは、おもしろい・・・。”

D先生 ： “今回は、LLMの出力に不安はありますが、とても楽しめそうですね。”



## ～ まとめ ～

### ・・・ ちょっと前の話のつづきです ・・・

D先生： “とってももったいないので、**「10Sの話（↓）」**をコピペしておきましょう。それにしても、最近は低レベルのJ国の人が、ずいぶん彼に絡んでいるらしいですね。”

![imageJRF3-1-9](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-9.jpg)

QEU:FOUNDER ： “ホント・・・。彼(↑V国人)を支持するC国の人にも絡んでいるJ国の大〇鹿者がいるらしいね。いつも彼のポストを読んでいますが、とくにJ国をDISっているわけじゃないです。J国に長く住んでいるからこそ語れる文化論であり、これをきちんと読めば、とても参考になるものばかりですよ。そもそも、J国は、これから「インバウンドだけで生きていく国」じゃないの？**大事なお客さんを差別してどうするの？**”

1. 嫉妬深い: 他人の成功や幸福に対して、強い嫉妬心を抱きやすい。 \n
2. 視野が狭い: 自分の意見や価値観に固執し、他の視点を受け入れにくい。  \n
3. 責任恐怖 : 責任を負うことを過度に恐れ、行動をためらう。  \n
4. 責任転嫁: 自分の失敗や問題を他人に押し付けて逃げようとする。  \n
5. 責任が曖昧: 失敗した際、誰の責任かが不明確になりがち。  \n
6. 差別が好き： 無意識に他者を差別し、偏見で扱う傾向がある。  \n
7. 先入観が好き: 新しい事実を無視して、既存の先入観に固執しやすい。  \n
8. 世間体ばかり気にする: 他人の目を過度に気にして、自分の行動を決める。  \n
9. 信念が薄い: 周囲の意見に左右されやすく、自分の信念が揺らぎやすい。  \n
10. 思考停止しやすい: 考えを深めず、既存のルールや習慣に依存する傾向がある。  \n

この「10S」は、個人や社会が抱える問題や課題を象徴しています。これらは自己改善や社会的な改善のための反省材料として活用でき、現状をより良くするための指針にもなります。私たちが「10S」に気づき、意識的に改善することで、より健全で前向きな社会を築くことができるでしょう。

D先生： “・・・でも、C国はわかるが、V国なのは、なぜ・・・！？”

[![MOVIE1](http://img.youtube.com/vi/OA_ZqXZmVpc/0.jpg)](http://www.youtube.com/watch?v=UdoLTVeT8R8 "NEW 2024 VinFast VF3 $20,000 Mini EV SUV - Everything We Know So Far")

QEU:FOUNDER ： “へっ・・・！？D先生とあろう方も知らないの？この新生のASEANカー・メーカーのこと？？VinFastは、2030年のASEANエリアにおいて、少なくともJ国のお山の大将（T社）よりもマーケット・シェアをもつと思うよ。ちなみに、2035年にはASEANのT国はガソリン車を販売しません。つまり、ASEAN諸国は一丸となり、EVの開発と生産を中心に、これから急速に工業力を持ち、お金持ちになります。一方、今後、**J国は、お上が強力に推進する「インバウンド（）」により、彼らからお金を恵んでもらうんです**。”

D先生： “むかしは、５ちゃんねるという便所の落書きでは、**「グエンが・・・」とか汚い言葉**で、V国実習生を差別していたJ国人は・・・。”

[![MOVIE2](http://img.youtube.com/vi/0R8Ql27cgWI/0.jpg)](http://www.youtube.com/watch?v=0R8Ql27cgWI "一連のクルド騒動は、維新も積極的な立憲叩きが発端")

QEU:FOUNDER ： “今後のJ国の人民は自分の立場をわきまえて、**「グエン様」と呼ばなくてはならない。それが、美しいJ国の精神です**。さっきのVinfastの話にもどろう・・・。この会社はイケているでしょ？”

![imageJRF3-1-10](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-10.jpg)

C部長 : “**U国出身の会社かあ・・・（笑）。** A国は、全力で車を買わなければいけないですね。 それにしても、**「莫国車廠」**って何ですか？”

QEU:FOUNDER ： “あの会社の所在地に、昔、莫王朝っていうのがあったんです。J国では1000人も知らない知識だと思うが・・・。あの地域の人にとって、**この王朝の存在は誇りなのですよ。**”

C部長 : “よく知ってますね。”

QEU:FOUNDER ： “まあね・・・。最近、カット・バ島に大きな橋がかかったので、なぜだろうと思っていたんです。この工場の所在地をみて、「なるほど」と思いました。車廠の場合には、塗装作業があるから住宅地から離したいんだよね。ただし、あそこは**名勝「ハロン湾」の付近にあるから環境には気を付けないと**・・・。これから、本題に入るよ。小生が、最近、ますます尊敬し始めたこの人（↓）の件・・・。。”

![imageJRF3-1-11](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-11.jpg)

C部長 : “**「〇〇主義の違い」**ですか・・・。”

![imageJRF3-1-12](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-12.jpg)

QEU:FOUNDER ： “**「認める」という言葉が、今回のポイントです。**”

現在、日本人の急務は「違い」を認めることだと思います。

1. 思い込みと現実の違い。 \n
2. 自分と他人の違い。 \n
3. 自国と他国の違い。 \n
4. 歴史と現在の違い。 \n
5. 現在と将来の違い。 \n
6. メディアが伝える世界と実際の世界の違い。 \n
7. 共産主義と資本主義の違い。 \n
8. 国と自分の違い。 \n

（他にもたくさんあります）

自分や自国の力を最大限に発揮するためには、これらの違いを認め、「自分がすごい」「自国がすごい」という盲点から脱却する以外に方法はありません。

QEU:FOUNDER ： “彼の言葉が深いのは、**「〇〇主義を認める」**といった点になります。このニュースを聞いて、じつは小生、このニュース（↓）を聞いて**「ひっくり返るほど」驚きました**。”

![imageJRF3-1-13](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-13.jpg)

C部長 : “あれ！？「〇〇主義」のV国の会社が、「△△主義」のIN国で車を生産するの！？しかも、2026年に量産化か・・・。”

QEU:FOUNDER ： “**ASEANは「〇〇主義」を認めたからこそ、このような経済発展が可能となりました。**それに引き換え・・・。”

![imageJRF3-1-14](/2024-09-02-QEUR23_BONSAITWC0/imageJRF3-1-14.jpg)

C部長 : “あらら・・・。お器が、小さいこと・・・。オホホ・・・。”

QEU:FOUNDER ： “小生は、J国が近い将来、その主要産業がインバウンドだけになって、**「まあ、（国の器には）ちょうど良い」**と思うようになったんですよ・・・(笑)。”
