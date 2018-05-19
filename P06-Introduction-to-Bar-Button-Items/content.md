---
title: "バーボタンアイテム入門"
slug: intro-bar-button
---

ノートアプリでは、ユーザーがノートを作成、編集、削除をできるようにしたいですね。そのために必要な、バーボタアイテムを紹介します。

`UIBarButtonItem`を使うことでナビゲーションバーにボタンを付け加えることができます。

![image showing bar button items](./images/barButtonItem.png)

３つのボタンが必要です。

まず、`ListNotesTableViewController`の中に、**create** バーボタンアイテムが必要です。

`DisplayNoteViewController`の中に、**save** ボタンと、**cancel** ボタンが必要です。

## ナビゲーションアイテム

`UIBarButtonItem`をナビゲーションバーに追加するために、ナビゲーションバーは _Navigation Item_ を持っている必要があります。

`Main.storyboard` を開くと、`ListNotesTableViewController`が _Navigation Item_ を持っていることが確認できるはずです。

![Existing Navigation Item](assets/existing_navigation_item.png)

`DisplayNoteViewController`のナビゲーションバーを見ると、_Navigation Item_ を持っていません。これを変更しましょう。

> [action]
`DisplayNoteViewController`　ナビゲーションバーに、_Navigation Item_ を追加しましょう。
>
![Add Navigation Item](assets/add_nav_item.png)

これで、二つのストーリーボードビューコントローラーは _Navigation Items_ を持っています。次に、`UIBarButtonItem`ビューを追加していきます。

# バーボタンアイテムを追加する

`ListNotesTableViewController`に _Create Note_ バーを追加しましょう。

> [action]
新しくバーボタンアイテムを追加します。
>
1. `UIBarButtonItem` を`ListNotesTableViewController`の右上に追加します。
1. `UIBarButtonItem` を選択します。
1. バーボタンアイテムが選択された状態で、_Attributes Inspector_ へ移動します。
1. _System Item_ を`Custom`から`Add`へ変更します。
>
![Add Create Bar Button Item](assets/add_create_bar_button_item.png)

後二つです！

`DisplayNoteViewController`に、saveとcancelボタンを加えましょう。上でやったものと同じプロセスです。

> [action]
`DisplayNoteViewController`にsaveとcancelボタンを追加します。
>
![Save Cancel Bar Button Items](assets/save_cancel_bar_button_items.png)

これで３つのバーボタンアイテムが追加されました。でも、今もしアプリを実行してボタンをタップしても、何も起こりません。

これから、ボタンをタップしたら期待通りの動きが起こるように設定していきます。

# バーボタンアイテムセグエ

バーボタンをタップするたびに、_push_ もしくは _pop_ ボタンが起きるようにしたいですね。

_Create Note_ ボタンの場合、`UIBarButtonItem` をタップしたら、`ListNotesTableViewController`が`DisplayNoteViewController`へ移動するように設定する必要があります。

これを実装しましょう！

> [action]
_Create Note_ バーボタンアイテムをタップしたときに実行されるセグエを作り、さらにIDを設定しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/create_add_segue_w_identifier.mp4)
>
ステップ:
>
1. _Create Note_ ボタンを選択します。プラス(+)ボタンのように見えているはずです。
1. _Create Note_ バーボタンを選択したまま、controlを押して`DisplayNoteViewController`へドラッグします。
1. ポップアップウィンドウで、_Action Segue_ の `Show` を選択してセグエを作成します。
>
セグエIDを設定しましょう。
>
1. `ListNotesTableViewController`と`DisplayNoteViewController`の間の新しいセグエを選択します。正しいセグエが選択されていたら、_Create Note_ バーボタンアイテムがハイライトされているはずです。
1. _Attributes Inspector_ へ移動します。
1. _Identifier_ 欄に`addNote`と入力します。

新しいIDを使って、`prepare(for:sender:)`をアップデートしましょう。

> [action]
`ListNotesTableViewController.swift`を開いて、`prepare(for:sender:)`をアップデートします。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "displayNote":
        print("note cell tapped")
>
    case "addNote":
        print("create note bar button item tapped")
>
    default:
        print("unexpected segue identifier")
    }
}
```
>
上のコードでは、if文ではなくswitch文を使っています。
>
それぞれのIDに対して、アウトプットを変えています。

今アプリを実行すると、_Create Note_ をタップしたときに`DisplayNoteViewController`へ移動し、デバッグコンソールにメッセージが表示されているはずです。

次に、`DisplayNoteViewController`のセグエを実装していきます。

# Unwind Segues

Save と Cancelボタンは`DisplayNoteViewController`を _pop(手放す)_ 機能を持つ必要があります。

このセグエは _pushe_ の代わりに _pops_ をするので、_unwind segue_ という方法を実装する必要があります。

_Unwind segue_ はビューコントローラーを _pop_ する役割をします。言い換えると、これは前に表示されていたビューコントローラーに表示を戻す役割をするのです。

例えば、_Create Note_ ボタンをタップしたとき、セグエを起動させて`DisplayNoteViewController` へプッシュします。そして、キャンセルボタンを押したとき、_unwind segue_ を使って`DisplayNoteViewController`からテーブルビューコントローラーに戻します。

![Notes Unwind Segue Example](./images/unwind-segue.png)

## セグエのメモリリーク

複数のビューコントローラを行き来するとき、メモリリークに気をつけましょう！

_Show_ セグエを起動するとき、ビューコントローラの新しいインスタンスが _push_ されてナビゲーションスタックの上に積まれます。つまり、もし _show_ だけを使っていると、次々にビューコントローラのインスタンスが新たに生成されて行くのです。そのままだと、アプリがメモリ不足で止まってしまう恐れがあります！

下のイメージは、_push_ をするときだけセグエを使っています。何回かセグエを実行した後、ナビゲーションスタックは`ListNotesTableViewController`や`DisplayNoteViewController`のインスタンスを持っているはずです。

![image showing no unwind segues](./images/no-unwind-segue.png)

ダメですね！これが _unwind segues_ を使わなければいけない理由です。

## Unwind Segueを実装する

`DisplayNoteViewController`に _unwind segues_ を実装しましょう。

_unwind segues_ を作る２つのステップ:

1. 戻ろうとしている目的地のビューコントローラに特別な`@IBAction`メソッドを作成します。今回のケースでは、テーブルビューコントローラーにこのメソッドを加えます。
1. ストーリーボードに _unwind segue_ を作成します。

順番通りにステップを踏むことが大切です。`@IBAction`メソッドがない状態では、_unwind segue_ を追加することができません。

まずはステップ１から始めましょう。`DisplayNoteViewController`から`ListNotesTableViewController`へ"unwind"します。つまり、目的地は、`ListNotesTableViewController`です。

> [action]
`ListNotesTableViewController`に、下の`@IBAction`を追加しましょう:
>
```
@IBAction func unwindWithSegue(_ segue: UIStoryboardSegue) {
>
}
```
>
`IBAction`の中には何もコードを加える必要はありません。_unwind segue_ を作るために必要なだけなのです。

ステップ２へ移ります。_unwind segues_ をcancelとsaveボタン両方に加えます。

> [action]
`Main.storyboard`に、_unwind segue_ を加え、IDを与えます。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/create_cancel_segue_w_identifier.mp4)
>
ステップ:
>
1. キャンセルボタンを選択します。
1. controlを押しながらドラッグし、オレンジのexitアイコンへ引っ張ります。
1. ポップアップで、ステップ１で作成した`IBAction`を選択します。
>
次に、_unwind segue_ にIDを加えます:
>
1. _unwind segue_ を選択します。
1. セグエを選択したままで、_Attributes Inspector_ へ移動します。
1. _Identifier_ 欄を`cancel`に設定します。

次に、同じステップをsaveボタンに対しても行います。ステップ１は必要ありません。

> [challenge]
`DisplayNoteViewController`のsaveボタンに対して、ステップ２を行いましょう。_unwind segue_ のIDを`save`に設定しましょう。
>
もし困ったら、いつでも前のセクションに戻ってcancelボタンの時はどうしたかを確認しましょう。

終わったら、`DisplayNoteViewController`にコードを加えて _unwind segue_ が実行される前に通知を送ります。加えて、設定したIDを使って、どちらのセグエが呼び出されたかを確認できるようにします。

> [action]
`DisplayNoteViewController.swift`　に下のコードを加えましょう。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>    
    switch identifier {
    case "save":
        print("save bar button item tapped")
>        
    case "cancel":
        print("cancel bar button item tapped")
>        
    default:
        print("unexpected segue identifier")
    }
}
```
>
上のコードは、`prepare(for:sender:)`を使って _unwind segue_  が起動したかどうかを知らせてくれます。`prepare(for:sender:)`メソッドはとても便利です。なぜなら、ビューコントローラが進む時も、戻る時もどちらにも使えるからです。

## アプリを実行する

このセクションでは、新しいセグエを設定しました。きちんと動くか確認しましょう！

> [action]
アプリをビルド・実行しましょう。テーブルビューコントローラーで、_Create Note_ をクリックしましょう。次に、cancel と save ボタンを押してみましょう。どちらのボタンも正常な動きをしているか確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/bar_button_item_segue_checkpoint.mp4)

次は、ノートアプリのロジックを実装していきます！
