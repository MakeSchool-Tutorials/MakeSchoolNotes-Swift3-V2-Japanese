---
title: "コアデータの統合"
slug: integrating-CoreData
---

We're at the beginning of the end. It's been quite a journey. In this section, we'll finish our _Notes_ app functionality by implementing persistence with our `CoreDataHelper`.
そろそろ終わりに近づいてきました。少し長い旅でしたね、このセクションでは、`CoreDataHelper`を使って、データが永続的に保存されるような機能を実装します。

# Retrieving Notes
# ノートを取得する

When a user first opens our app, we want to retrieve and display all of the user's existing notes. We can do this by retrieve our existing notes and updating our `notes` array in `ListNotesTableViewController` at app launch.
ユーザーがアプリを開けるとき、存在するノートを全て表示させたいですね。アプリが起動するときに、ノートを取得し`ListNotesTableViewController`の`notes`配列をアップデートすることでそれが可能です。

Let's go ahead and implement this with our `CoreDataHelper`!
`CoreDataHelper`を使って実装しましょう！

> [action]
In `ListNotesTableViewController`, update the method `viewDidLoad()` to retrieve the user's previously existing notes:
`ListNotesTableViewController`で、`viewDidLoad()`メソッドをアップデートしてノートを取得しましょう。
>
```
override func viewDidLoad() {
    super.viewDidLoad()
>    
    notes = CoreDataHelper.retrieveNotes()
}
```

The `viewDidLoad()` of our `ListNotesTableViewController` happens roughly when the app first launches. We can use our `CoreDataHelper` to retrieve the user's previously existing notes and populate our `notes` array.
`ListNotesTableViewController`の`viewDidLoad()`はアプリが初めて起動したときに呼び出されます。`CoreDataHelper`を使ってノートを取得し、`notes`配列を生成します。

Next, we'll also update our _unwind segue_ to retrieve our updated notes.
次は、_unwind segue_ をアップデートして、更新されたノートを取得します。

> [action]
In `ListNotesTableViewController`, update the _unwind segue_ method `unwindWithSegue(_:)` to the following:
`ListNotesTableViewController`で、_unwind segue_ メソッドの`unwindWithSegue(_:)`を書き換えます:
>
```
@IBAction func unwindWithSegue(_ segue: UIStoryboardSegue) {
    notes = CoreDataHelper.retrieveNotes()
}
```

Now, each time the user taps the save or cancel bar button item in `DisplayNoteViewController`, we update our `notes` array in `ListNotesTableViewController`. Our `didSet` property observer will take care of reloading our table view. In combination, we can be sure our table view data is always synced with our `NSManagedObjectContext`.
これで、`DisplayNoteViewController`でユーザーがsaveもしくはcancelボタンを押すたびに、`ListNotesTableViewController`の`notes`配列がアップデートされます。`didSet`プロパティが自動的にテーブルビューを書き換えます。`NSManagedObjectContext`とテーブルビューデータが常に一致しているかを確かめることができます。

# Deleting Notes
# ノートを削除する

We'll also need to update our delete note functionality.
ノートの削除機能も実装しましょう。

> [action]
In `ListNotesTableViewController`, update the table view data source method `tableView(_:commitEditingStyle:forRowAtIndexPath:)` to use our _Core Data_ helper:
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
In the code above, we retrieve the note to be deleted corresponding to the selected index path. Then we use our _Core Data_ helper to delete the selected note. Last we update our `notes` array to reflect the changes in our `NSManagedObjectContext`.
上のコードでは、index pathに基づいて対応する削除するべきノートを取得しています。そして _Core Data_ ヘルパーを使って、ノートを削除します。最後に`notes`配列をアップデートして`NSManagedObjectContext`の変更を反映しています。

# Updating DisplayNoteViewController
# DisplayNoteViewControllerをアップデート

Last, we'll need to update our functionality in `DisplayNoteViewController` for creating new notes or modifying existing ones. Once again, let's use our `CoreDataHelper` to update our code.
最後に、`DisplayNoteViewController`の機能をアップデートして新しいノートの作成、既存のノートの編集ができるようにします。`CoreDataHelper`をここでも使いますよ。

> [action]
In `DisplayNoteViewController`, update `prepare(for:sender:)` to use our `CoreDataHelper`:
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
We update our `prepare(for:sender:)` method to make use of our `CoreDataHelper`. Since we're retrieving notes in our `ListNotesTableViewController` _unwind segue_, we no longer need to reference our segue's destination controller.
`prepare(for:sender:)`をアップデートして`CoreDataHelper`を使いました。`ListNotesTableViewController` _unwind segue_ のノートを取得しているので、セグエの目的地の参照はいりません。

# Running the App
# アプリを実行する

Wooooooooo! We're done. Phew... I'm just as happy as you are. Congrats, you've just finishing building a fully functioning _Notes_ app with _Core Data_.
できましたね！！おめでとうございます！_Core Data_ を使って _ノート_ アプリができました！
