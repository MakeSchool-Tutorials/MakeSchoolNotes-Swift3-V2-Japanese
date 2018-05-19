---
title: "ノートの削除"
slug: deleting-notes
---

In this section, we'll implement the functionality for deleting notes.
このセクションでは、ノートの削除を実装します。

We'll use the built-in functionality of `UITableView` for deleting objects. It's super easy! (Really)
オブジェクトを削除するために、`UITableView`のビルトイン機能を使います。簡単ですよ！

> [action]
In `ListNotesTableViewController`, add the following method with your other table view data source methods:
`ListNotesTableViewController`で、下のコードを加えましょう。
>
```
override func tableView(_ tableView: UITableView, commit editingStyle: UITableViewCellEditingStyle, forRowAt indexPath: IndexPath) {
    if editingStyle == .delete {
        notes.remove(at: indexPath.row)
    }
}
```
>
In the code above, we override the table view data source method `tableView(_:commit:forRowAt:)` to implement the table view's built-in slide-to-delete functionality. Within the `tableView(_:commit:forRowAt:)` method, we check for the delete editing mode and delete the note at the corresponding index path.
上のコードでは、`tableView(_:commit:forRowAt:)` というデータソースメソッドを上書きして、テーブルビューの削除機能を実装しています。`tableView(_:commit:forRowAt:)`メソッドの中で、削除編集モードを確認し、index pathに対応するノートを削除しています。

That was easy!
簡単でしたね！！

## Running the App
## アプリを実行する

We've just finished implementing the functionality for deleting notes. Let's create a few notes and try deleting them to make sure everything works.
ノートの削除を実装しました。ノートを作って消してみましょう。

> [action]
Build and run the app. Create a few new notes and try deleting them.
アプリをビルド・実行しましょう。ノートを作って消してみましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p10_deleting_notes/delete_note_checkpoint.mp4)

In the next section, we'll introduce _Core Data_ and learn how to persist our note data between app launches.
次のセクションでは、_Core Data_ を紹介します！
