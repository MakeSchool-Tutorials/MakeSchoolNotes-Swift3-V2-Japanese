---
title: "ノートを表示する"
slug: displaying-notes
---

このセクションでは、ユーザが既に作成されたノートを見たり編集したりできるように機能を実装します。

まず、`DisplayNoteViewController`に`note`プロパティを加えることから始めます。`ListNotesTableViewController`から`DisplayNoteViewController`に存在するノートを移す必要があります。

> [action]
`DisplayNoteViewController`のトップに、下のプロパティを付け加えます。
>
```
class DisplayNoteViewController: UIViewController {
>    
    // MARK: - Properties
>    
    var note: Note?
>
    // ...
}
```
>
`note`に、オプショナルの`Note`型を持たせています。この値は`nil`の可能性があるからです。
`note`の二つの可能性:
>
1. 存在するノートを見たり表示させる時、`note`プロパティはnilではない値になる。
1. 新しくノートを作る時、`note`は`nil`になる。

# ノートのデータをセグエを通して移す

`prepare(for:sender:)`を使って新しく作ったノートを`DisplayNoteViewController`から`ListNotesTableViewController`へ移しました。

存在するノートを表示したり編集したりする場合、同じことを逆方向にする必要があります。ノートを`ListNotesTableViewController`から`DisplayNoteViewController`へ移すのです。

さてコードを実装しましょう。

> [action]
`ListNotesTableViewController`で、`prepare(for:sender:)` をアップデートしましょう。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "displayNote":
        // 1
        guard let indexPath = tableView.indexPathForSelectedRow else { return }
>
        // 2
        let note = notes[indexPath.row]
        // 3
        let destination = segue.destination as! DisplayNoteViewController
        // 4
        destination.note = note
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
ステップ:
>
1. `UITableView`のプロパティである`indexPathForSelectedRow`を使って、テーブルビューの選択された列のindex pathを取得します。これを`notes`配列からデータを取り出すのに使用します。
1. index pathを使って、ノートを取り出します。
1. セグエの目的地のビューコントローラを参照します。これにより、`DisplayNoteViewController`の`note`プロパティを設定できます。
1. 目的地のビューコントローラーの参照を使って、`note`プロパティを選択したノートに設定します。

# ノートを表示する

ノートのデータを移すことに成功したので、ノートを表示することができます。

`DisplayNoteViewController`では、`viewWillAppear()`にコードを付け加えてストーリーボードのダミーテキストを削除しました。ノートを表示するには、このメソッドを書き換えてテキストビューとテキストフィールドにノートのデータを表示する必要があります。

> [action]
`DisplayNoteViewController`で、`viewWillAppear()`を書き換えます。
>
```
override func viewWillAppear(_ animated: Bool) {
    super.viewWillAppear(animated)
>
    // 1
    if let note = note {
        // 2
        titleTextField.text = note.title
        contentTextView.text = note.content
    } else {
        // 3
        titleTextField.text = ""
        contentTextView.text = ""
    }
}
```
>
`viewWillAppear()`メソッドの中で、`note`プロパティのノートを確認して、ユーザーが存在するノートを表示させようとしているのか、新しいノートを作ろうとしているのかを決めます。
>
ステップ:
>
1. 存在するノートの`note`プロパティをチェックします。（if-letを使っています)
1. `note`プロパティがnilでなければ、`DisplayNoteViewController`がテキストフィールドとテキストビューをノートの中身に合わせて生成します。
1. `note`プロパティが`nil`だったら、`DisplayNoteViewController`がlorem ipsumのダミーテキストを削除して、ノートの作成が簡単になるようにします。

# 編集を保存する

存在するノートを表示するコードを無事に実装しました。しかし、ノートを保存しようとすると、このコードは新しいノートを作成してしまいます。

どのように編集が保存できるかを考えて見ましょう。

`DisplayNoteViewController`の`prepare(for:sender:)`のロジックを確認しましょう。

```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }

    switch identifier {
    case "save":
        let note = Note()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()

        let destination = segue.destination as! ListNotesTableViewController
        destination.notes.append(note)

    case "cancel":
        print("cancel bar button item tapped")

    default:
        print("unexpected segue identifier")
    }
}
```

上のコードでは、saveボタンは常に新しいノートを作り、`ListNotesTableViewController`の`notes`配列に付け加えてしまっています。

これを書き換えて、存在するノートに変更を保存するようにしましょう。`note`プロパティを使って、新しいノートを作るのか、既に存在しているノートに保存するのかを決めます。

> [action]
`DisplayNoteViewController`で、`prepare(for:sender:)`を書き換えましょう。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // 1
    guard let identifier = segue.identifier,
        let destination = segue.destination as? ListNotesTableViewController
        else { return }
>
    switch identifier {
    // 2
    case "save" where note != nil:
        note?.title = titleTextField.text ?? ""
        note?.content = contentTextView.text ?? ""
>
        destination.tableView.reloadData()
>
    // 3
    case "save" where note == nil:
        let note = Note()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()
>
        // 4
        destination.notes.append(note)
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
上のコードがやっていること:
>
1. if-let文を加えて、セグエの目的地が`ListNotesTableViewController`タイプかを確認します。これでセグエの目的地のビューコントローラーが安全にtype castされて、Switch文を使うことができるようになります。
1. `save`を二つの条件に分けています。初めの方は、存在するノートを保存する場合です。`DisplayNoteViewController`の`notes`を確認することで、ノートが存在しているかを確認することができるのです。もしノートが存在していたら、プロパティを設定した上で`ListNotesTableViewController`のテーブルビューをリロードします。
1. `note`プロパティが`nil`だったら、ユーザーは新しいノートを作っていることになります。このケースでは、前回作ったコードを再利用して新しいノートを作るようにします。
1. guardステートメントで目的地のビューコントローラを型変換しているので、このビューコントローラをdowncastingすることなく使うことができます。

<!-- break -->

> [info]
存在するノートを保存するケースでは、変更されたノートを`ListNotesTableViewController`に移す必要はありません。これは、Swiftではclassタイプは参照によって移すことができるからです。
>
つまり、どちらのビューコントローラーも同じクラスインスタンスを参照しているのです。もしクラスインスタンスがあるビューコントローラーで更新されたら、他のビューコントローラーからでもその更新されたオブジェクトにアクセスできるのです。
>

## アプリを実行する

このセクションでは、既に存在するノートを確認したり編集したりすることができるようになりました。全てちゃんと機能しているか確認しましょう！

> [action]
アプリをビルド・実行しましょう。新しいノートを作ったり、編集ができるかを確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p09_displaying_notes/editing_note_checkpoint.mp4)

次のセクションで、ノートの削除を実装します。
