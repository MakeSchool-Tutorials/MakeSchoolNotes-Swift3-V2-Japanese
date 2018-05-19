---
title: "ナビゲーションコントローラー入門"
slug: intro-nav-controller
---

`UINavigationController`はビューコントローラのサブクラスです。複数のビューコントローラーを行き来する機能を実装するのに必要です。また、それ以外にも様々な機能を持っています。

Appleのカレンダーアプリを観察してナビゲーションコントローラの役割を見てみましょう。

年間のカレンダーを表示します。

![calendar app year view](./images/year.png)

トップの半透明のバーをは、_navigation bar_ と呼ばれています。_navigation bar_ は　`UINavigationBar`タイプを持っています。

ある月をクリックしてみましょう。ナビゲーションコントローラーが年間から月間のビューに変わりましたね。月間のビューコントローラーが年間の上に　_push_　されたのです。

![calendar app month view](./images/month.png)

トップのナビゲーションバーを見ると、戻るボタンがあります。月間のビューコントローラーの下に年間のビューコントローラーがあるので、戻るボタンを押すと自動的に　_pop_　バックをして前に表示されていた（年間の）ビューコントローラーに戻ります。

`UINavigationController`は複数のビューコントローラを行き来するのにとても便利ですね。

月間から、ある日にちをタップすると、ナビゲーションコントローラーが現在の _navigation stack_ の上に週間のカレンダーを _push_ します。

![calendar app day view](./images/day.png)

ナビゲーションコントローラースタックは、下のビューコントローラーを持っていることになります。

1. 週間のビューコントローラー
1. 月間のビューコントローラー
1. 年間のビューコントローラー

それぞれのビューコントローラーは _push_ されてナビゲーションスタックの上に配置され、それまでに表示していたビューコントローラーの上に重なります。これで、戻るボタンを押せば簡単にいつでも戻ることができるのです。

例えば、今戻るボタンを押せば、この週間のビューコントローラーはナビゲーションスタックから消えて、次に配置されていたビューコントローラーが表示されます。（つまり月間のビューコントローラーです）

さて、私たちの _ノート_ アプリでは、二つのビューコントローラーがあります。

１つ目は、`ListNotesTableViewController`。これはいつもナビゲーションスタックの一番下に配置されます。ノートの全てのリストを表示します。

次に、それぞれのノートを表示するビューコントローラーが必要です。`DisplayNoteViewController`がその役割を果たします。

ユーザーがノートを見たり作ったりしたいとき、`DisplayNoteViewController`がナビゲーションスタックの上にプッシュされます。ここではノートを作ったり、編集したり、見ることができます。

まず、`UINavigationController`を実装していきます。

# ナビゲーションコントローラを実装する

ナビゲーションコントローラーはたくさんの機能を持っています。初心者に優しく、簡単に設定ができます。


> [action]
`Main.storyboard`を開き、ナビゲーションコントローラーを追加しましょう。
>
1. 今使っているテーブルビューコントローラーを選択します。
1. テーブルビューコントローラーを選択した状態で、`Editor > Embed In > Navigation Controller` を選択します。

>
![Embed In Navigation Controller](assets/embed_in_navigation_controller.png)

ナビゲーションコントローラが埋め込まれました。

![Storyboard Navigation Controller](assets/storyboard_nav_controller.png)

## タイトルを追加する

`UINavigationController`は、タイトルを追加することができます。

このテーブルビューコントローラーは、ノートのリストを表示しますね。これに合わせてタイトルを付け加えましょう。

> [action]
タイトルを付け加えよう:
>
1. _Navigation Item_ を選択します。
1. _Attributes Inspector_ へ移動します。
1. _Title_ に、`Notes`を書き込みます。
>
![Set Nav Bar Title](assets/set_nav_bar_title.png)

## アプリを実行する

ナビゲーションバーを埋め込みましたね。これでページを移動することができるようになりました。アプリを起動して正常に動くか確かめましょう！

この時点で、アプリは下のように見えるはずです。

![Nav Bar Checkpoint](assets/nav_bar_checkpoint.png)

さあ、どんどん進みましょう。
