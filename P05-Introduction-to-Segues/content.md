---
title: "セグエ入門"
slug: intro-segues
---

`DisplayNoteViewController`を設定して、`UINavigationController`の機能を実装することができるようになりました。

ナビゲ＝ションコントローラーは _segues(セグエ)_ を使って、ビューを移動します。_segues(セグエ)_ は 前のページで説明した _push_ と _pop_ を可能にします。

# セグエを作る

_ノート_ アプリでは、`ListNotesTableViewCell`をタップすると、`DisplayNoteViewController`へ移動するようにしたいですね。

> [info]
セグエはプログラムを書いて実装することもできますが、今回は、ストーリーボードを使って実装します。

<!-- break -->

> [action]
`Main.storyboard`で、セグエを作って、ユーザーがセルをタップしたら実行されるようにしましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p05_introduction_to_segues/cell_tap_segue.mp4)
>
ステップ:
>
1. テーブルビューコントローラーで、ストーリーボードセルを選択します。
1. 選択をしたままで、controlを押しながら`DisplayNoteViewController`へドラッグします。
1. ポップアップが出てくるので、_Selection Segue_ の `Show` を選択します。

セグエを作った後、テーブルビューコントローラーとDisplayNoteViewControllerが繫がりました。

![New Segue](assets/new_segue.png)

`UINavigationController`を`DisplayNoteViewController`に追加した時、サブビューが自動的に再配置されたのに気づきましたか。_auto-layout_ を設定したためです。楽ですね！

## セグエIDを追加する

セグエIDはテーブルビューセルIDと似ています。Swiftでコードを書くときにセグエを使うことができるようにするためです。

セグエIDを追加しましょう。

> [action]
セグエIDを追加します。
>
![Set Segue Identifier](assets/set_segue_identifier.png)
>
ステップ:
>
1. セグえをクリックします。
1. _Attributes Inspector_ へ移動します。
1. _Identifier_ 欄 を`displayNote`と入力します

# セグエを使うための準備

`prepare(for:sender:)`と呼ばれる`UIViewController`のメソッドは、新しいビューコントローラーが _push_ される直前に実行されます。

> [info]
自分で`prepare(for:sender:)`を呼び出してはいけません。`prepare(for:sender:)`はセグエが実行される直前に自動的に呼ばれるからです。

これで、セグエを準備することができます。

さあ、実験してみましょう！

> [action]
`ListNotesTableViewController`を開き、下のコードを加えます。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // 1
    guard let identifier = segue.identifier else { return }
>
    // 2
    if identifier == "displayNote" {
        print("Transitioning to the Display Note View Controller")
    }
}
```
>
上のコードでしていること
>
1. セグエがIDを持っているか確認する
1. もしそうであれば、そのIDが`displayNote`と同じかを確認する。もしそうであれば、デバッグコンソールに状況を表示する。

## アプリを実行する

このセクションでは、`ListNotesTableViewController`のセルをユーザーがタップしたときに呼び出されるセグエを設定しました。さらに`prepare(for:sender:)`を実装して、このセグエが呼び出されたときにデバッグコンソールに表示されるようにコードを加えました。

このセグエがきちんと動くか確認しましょう！

> [action]
アプリをビルド・実行しましょう。テーブルビューコントローラーで、ノートセルをタップしましょう。backボタンを押すと前の画面に戻ります。`prepare(for:sender:)`で実装したコードがデバッグコンソールに表示されているか確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p05_introduction_to_segues/segue_checkpoint.mp4)

さあ、次のページへ進みましょう！
