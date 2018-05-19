---
title: "コアデータの統合"
slug: integrating-CoreData
---

そろそろ終わりに近づいてきました。少し長い旅でしたね、このセクションでは、`CoreDataHelper`を使って、データが永続的に保存されるような機能を実装します。

# ノートを取得する

ユーザーがアプリを開けるとき、存在するノートを全て表示させたいですね。アプリが起動するときに、ノートを取得し`ListNotesTableViewController`の`notes`配列をアップデートすることでそれが可能です。

`CoreDataHelper`を使って実装しましょう！

> [action]
`ListNotesTableViewController`で、`viewDidLoad()`メソッドをアップデートしてノートを取得しましょう。
>
```
override func viewDidLoad() {
    super.viewDidLoad()
>    
    notes = CoreDataHelper.retrieveNotes()
}
```

`ListNotesTableViewController`の`viewDidLoad()`はアプリが初めて起動したときに呼び出されます。`CoreDataHelper`を使ってノートを取得し、`notes`配列を生成します。

次は、_unwind segue_ をアップデートして、更新されたノートを取得します。

> [action]
`ListNotesTableViewController`で、_unwind segue_ メソッドの`unwindWithSegue(_:)`を書き換えます:
>
```
@IBAction func unwindWithSegue(_ segue: UIStoryboardSegue) {
    notes = CoreDataHelper.retrieveNotes()
}
```

これで、`DisplayNoteViewController`でユーザーがsaveもしくはcancelボタンを押すたびに、`ListNotesTableViewController`の`notes`配列がアップデートされます。`didSet`プロパティが自動的にテーブルビューを書き換えます。`NSManagedObjectContext`とテーブルビューデータが常に一致しているかを確かめることができます。

# ノートを削除する

ノートの削除機能も実装しましょう。

> [action]
`ListNotesTableViewController`で、 _Core Data_ を使って`tableView(_:commitEditingStyle:forRowAtIndexPath:)`を更新します。
>
```
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        let noteToDelete = notes[indexPath.row]
        CoreDataHelper.delete(note: noteToDelete)
>
        notes = CoreDataHelper.retrieveNotes()
    }
}
```
>
上のコードでは、index pathに基づいて対応する削除するべきノートを取得しています。そして _Core Data_ ヘルパーを使って、ノートを削除します。最後に`notes`配列をアップデートして`NSManagedObjectContext`の変更を反映しています。

# DisplayNoteViewControllerをアップデート

最後に、`DisplayNoteViewController`の機能をアップデートして新しいノートの作成、既存のノートの編集ができるようにします。`CoreDataHelper`をここでも使いますよ。

> [action]
`DisplayNoteViewController`で、`prepare(for:sender:)` をアップデートして`CoreDataHelper`を使います
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "save" where note != nil:
        note?.title = titleTextField.text ?? ""
        note?.content = contentTextView.text ?? ""
        note?.modificationTime = Date()
>
        CoreDataHelper.saveNote()
>
    case "save" where note == nil:
        let note = CoreDataHelper.newNote()
        note.title = titleTextField.text ?? ""
        note.content = contentTextView.text ?? ""
        note.modificationTime = Date()
>
        CoreDataHelper.saveNote()
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
`prepare(for:sender:)`をアップデートして`CoreDataHelper`を使いました。`ListNotesTableViewController` _unwind segue_ のノートを取得しているので、セグエの目的地の参照はいりません。

# アプリを実行する

できましたね！！おめでとうございます！_Core Data_ を使って _ノート_ アプリができました！
