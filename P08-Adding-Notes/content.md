---
title: "ノートを追加する"
slug: adding-notes
---

データモデルをセットアップしたので、ノートを追加する機能を実装しましょう！まず初めに、初期設定が必要です。

# View Controller セットアップ

テキストフィールドとテキストフィールドに対して、`IBOutlet`を作ります。これらの`IBOutlet`は、テキストフィールドとテキストビューをコード上で参照するのに必要です。

> [challenge]
_Assistant Editor_ を使って、`DisplayNoteViewController`の中のテキストフィールドとテキストビューに`IBOutlet`を追加しましょう。
>
1. テキストフィールドの`IBOutlet`を`titleTextField`と名付けます。
1. テキストビューの`IBOutlet`を`contentTextView`と名付けます。

次に、`IBOutlet`を使って、テキストビューにデフォルトで表示されているテキストを削除しましょう。

## Lorem　Ipsumを削除する

今、_Create Note_ をタップすると、下のように表示されるはずです。

![Filler Text](assets/filler_text.png)

lorem ipsumのテキストで埋められているはずです。

これは正しくありませんね、、ユーザーが新しいノートを作るとき、テキストビューは空であるべきです。

直しましょう。

> [action]
`DisplayNoteViewController`で、`viewDidLoad`メソッドの下に、コードを追加しましょう。
>
```
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
>    
    titleTextField.text = ""
    contentTextView.text = ""
}
```
>
`viewWillAppear(_:)`メソッドは、ビューコントローラのライフサイクルメソッドで、このビューがスクリーンに表示される直前に呼び出されるものです。
>
上のコードで、テキストフィールドとテキストビューの両方のテキストプロパティをセットしています。これでlorem ipsumテキストがなくなり、空のテキストビューになりました。

# 新しいノートを作成する

これで、初めてのノートを作成する準備ができました。

> [action]
`DisplayNoteViewController`で、`prepare(for:sender:)`を下のコードに書き換えましょう。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "save":
        // 1
        let note = Note()
>
        // 2
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
>
        // 3
        note.modificationTime = Date()
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
上のコードでは、`save`のSwitch文を下のように書き換えています。
>
1. `Note`オブジェクトを初期化する
1. 新しいノートのタイトルと本文を、対応するテキストフィールドとテキストビューの値に書き換える。もしどちらかの値が`nil`だった場合、デフォルトの値として空のString値を与える。
1. 新しいノートの最終編集日時を現在の日時に設定する。

## ノートを保存する

これで新しいノートが作成できましたが、まだ保存されていません。保存するために、このデータを`ListNotesTableViewController`の`notes`配列に追加する必要があります。セグエを使ってビューコントローラー同士でデータを受け渡しします。

> [action]
`DisplayNoteViewController`で、`prepare(for:sender:)`に下のコードを書き換えます。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "save":
        let note = Note()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()
>
        // 1
        let destination = segue.destination as! ListNotesTableViewController
        // 2
        destination.notes.append(note)
>
    case "cancel":
        print("キャンセルボタンがタップされました")
>
    default:
        print("予期しないsegue identifierです")
    }
}
```
>
ステップ:
>
1. それぞれのセグエは二つのビューコントローラを繋ぎ、ソースと目的地のビューコントローラの情報を参照しています。しかし、目的地のビューコントローラーのプロパティにアクセスするためには、目的地のビューコントローラを`ListNotesTableViewController`タイプに変換する必要があります。
`ListNotesTableViewController`を参照することで、`notes` 配列に新しいノートを追加することができます。

## テーブルビューのデータソースを再読み込みする

saveバーボタンをタップすると、新しく作られたノートが`ListNotesTableViewController`のnote配列に追加されます。最後に、`UITableView`に追加されたことを知らせる必要があります。。`UITableView`のメソッドの`reloadData()`を使います。

> [info]
テーブルビューは、テーブルビューセルUIを自動的にアップデートしてくれません。もしテーブルビューをアップデートしたい場合は、テーブルビューに変更を通知する必要があるのです。
>
変更を通知すると、テーブルビューは再度テーブルビューセルを計算します。

でも、いつテーブルビューにそのデータを知らせればよいのでしょう？

Property observer(プロパティオブザーバー)の出番です。Property Observerを使うと、プロパティが変更されたときに実行されるコードを書くことができます。


> [action]
`ListNotesTableViewController`で、`notes`配列を下のように書き換えましょう。
>
```
var notes = [Note]() {
    didSet {
        tableView.reloadData()
    }
}
```
>
`notes`配列にプロパティオブザーバーを作成するためにシンタックスを加えました。`didSet`は`notes`プロパティを見張っています。もしプロパティが変わったら、`didSet`のブロックの中身が実行させるのです。
>
これはとても便利ですね。ノートが追加されるたびに、`notes`配列に追加されていきます。

## アプリを実行する

`ListNotesTableViewController`のnotes配列にノートを追加する機能を実装しました。`prepare(for:sender:)`を使って、ビューコントローラ間でデータを受け渡し、プロパティオブザーバーを使って、新しいノートが作成されたときにテーブルビューがアップデートされるようにしました。

さあ、全てうまく機能しているか確かめましょう！

> [action]
アプリをビルドして実行します。_Create Note_ ボタンを押して新しいノートを作り、保存しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p08_adding_notes/create_note_checkpoint.mp4)

新しいノートを作ることができました。さて、作成されたノートをクリックしたら何が起きますか＞

![Empty Existing Note](assets/empty_existing_note.png)

空のノートが出てきてしまいました！
次のセクションでこれを直しましょう。

> [info]
”アプリを再起動するたびにノートがリセットされるのはなぜ？”と思っている人もいるかもしれません。
>
これは、まだノートのデータを永続的に使う設定をしていないからです。この設定は、この先のチュートリアルの中で出てきます。
