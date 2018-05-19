---
title: "ノートの削除"
slug: deleting-notes
---

このセクションでは、ノートの削除を実装します。

オブジェクトを削除するために、`UITableView`のビルトイン機能を使います。簡単ですよ！

> [action]
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
上のコードでは、`tableView(_:commit:forRowAt:)` というデータソースメソッドを上書きして、テーブルビューの削除機能を実装しています。`tableView(_:commit:forRowAt:)`メソッドの中で、削除編集モードを確認し、index pathに対応するノートを削除しています。

簡単でしたね！！

## アプリを実行する

ノートの削除を実装しました。ノートを作って消してみましょう。

> [action]
アプリをビルド・実行しましょう。ノートを作って消してみましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p10_deleting_notes/delete_note_checkpoint.mp4)

次のセクションでは、_Core Data_ を紹介します！
