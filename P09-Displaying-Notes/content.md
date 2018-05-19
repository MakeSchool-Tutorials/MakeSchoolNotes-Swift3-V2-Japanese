---
title: "ノートを表示する"
slug: displaying-notes
---

In this section, we'll implement the functionality for users to view and edit existing notes.
このセクションでは、ユーザが既に作成されたノートを見たり編集したりできるように機能を実装します。

First, we'll start by adding a `note` property to our `DisplayNoteViewController`. We'll need this to pass an existing note from the `ListNotesTableViewController` to the `DisplayNoteViewController` to be displayed.

まず、`DisplayNoteViewController`に`note`プロパティを加えることから始めます。`ListNotesTableViewController`から`DisplayNoteViewController`に存在するノートを移す必要があります。

> [action]
In `DisplayNoteViewController`, add the following property at the top of your view controller:
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
We set our `note` to have a type of optional `Note`, or `Note?`, because it's value can potentially be `nil`. There are two cases for our `note` property:
`note`に、オプショナルの`Note`型を持たせています。この値は`nil`の可能性があるからです。
`note`の二つの可能性:
>
1. When displaying or view an existing note, our `note` property will be a non-nil value.
1. 存在するノートを見たり表示させる時、`note`プロパティはnilではない値になる。
1. When creating a new note, our `note` property will be `nil`.
1. 新しくノートを作る時、`note`は`nil`になる。

# Passing Note Data Via Segues
# ノートのデータをセグエを通して移す

Previously, we used `prepare(for:sender:)` to pass a newly created note from `DisplayNoteViewController` back to our `ListNotesTableViewController`.
`prepare(for:sender:)`を使って新しく作ったノートを`DisplayNoteViewController`から`ListNotesTableViewController`へ移しました。

To display or edit an existing note, we'll need to do the same thing in the opposite direction. We'll need to pass an existing note in the `ListNotesTableViewController` to our `DisplayNoteViewController`.
存在するノートを表示したり編集したりする場合、同じことを逆方向にする必要があります。ノートを`ListNotesTableViewController`から`DisplayNoteViewController`へ移すのです。

Let's implement this code now.
さてコードを実装しましょう。

> [action]
In `ListNotesTableViewController`, update `prepare(for:sender:)` with the following code:
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
Step-by-step:
ステップ:
>
1. Get a reference to the index path of the selected row in the table view using the `UITableView` property named `indexPathForSelectedRow`. We'll use this next to retrieve the correct note in our `notes` array.
1. `UITableView`のプロパティである`indexPathForSelectedRow`を使って、テーブルビューの選択された列のindex pathを取得します。これを`notes`配列からデータを取り出すのに使用します。
1. Retrieve the selected note using the index path from the previous step.
1. index pathを使って、ノートを取り出します。
1. Get a reference and type cast our segue's destination view controller. This will allow us to set the `note` property of the `DisplayNoteViewController` in the next step.
1. セグエの目的地のビューコントローラを参照します。これにより、`DisplayNoteViewController`の`note`プロパティを設定できます。
1. Using the reference to destination view controller, set the `note` property to the selected note.
1. 目的地のビューコントローラーの参照を使って、`note`プロパティを選択したノートに設定シアmす。

# Displaying The Note
# ノートを表示する

With our note data successfully passed between view controllers, we can display the existing note.
ノートのデータを移すことに成功したので、ノートを表示することができます。

Previously in our `DisplayNoteViewController`, we added code in our `viewWillAppear()` to remove our storyboard's filler text. To display our existing note, we'll need to update this method to populate the text field and text view (if the note already exists!)
`DisplayNoteViewController`では、`viewWillAppear()`にコードを付け加えてストーリーボードのダミーテキストを削除しました。ノートを表示するには、このメソッドを書き換えてテキストビューとテキストフィールドにノートのデータを表示する必要があります。

> [action]
In `DisplayNoteViewController`, update `viewWillAppear()` with the following code:
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
In our `viewWillAppear()` method, we check for an existing note in the `note` property to determine whether the user is displaying an existing note or creating a new one.
`viewWillAppear()`メソッドの中で、`note`プロパティのノートを確認して、ユーザーが存在するノートを表示させようとしているのか、新しいノートを作ろうとしているのかを決めます。
>
Step-by-step:
ステップ:
>
1. Check the `note` property for an existing note using if-let optional binding.
1. 存在するノートの`note`プロパティをチェックします。（if-letを使っています)
1. If the `note` property is non-nil, then our `DisplayNoteViewController` populates the text field and text view with the content of the existing note.
1. `note`プロパティがnilでなければ、`DisplayNoteViewController`がテキストフィールドとテキストビューをノートの中身に合わせて生成します。
1. If the `note` property is `nil`, then our `DisplayNoteViewController` clear the storyboard's lorem ipsum filler text so the user can quickly begin taking notes.
1. `note`プロパティが`nil`だったら、`DisplayNoteViewController`がlorem ipsumのダミーテキストを削除して、ノートの作成が簡単になるようjにします。

# Saving Edits
# 編集を保存する

We've successfully implemented the code for displaying an existing note. However, when we try to edit a note, our code creates a new note, instead of editing the existing one.
存在するノートを表示するコードを無事に実装しました。しかし、ノートを保存しようとすると、このコードは新しいノートを作成してしまいます。

Next, let's figure out how to save edits to existing notes.
どのように編集が保存できるかを考えて見ましょう。

Let's look at our `DisplayNoteViewController` logic in `prepare(for:sender:)` :
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

In the code above, tapping the save bar button item **always** creates a new note and appends it to the `notes` array of our `ListNotesTableViewController`.
上のコードでは、saveボタンは常に新しいノートを作り、`ListNotesTableViewController`の`notes`配列に付け加えてしまっています。

Let's update our code to account for saving edits to existing notes. We can use the `note` property to determine whether we're saving a new or existing note.
これを書き換えて、存在するノートに変更を保存するようにしましょう。`note`プロパティを使って、新しいノートを作るのか、既に存在しているノートに保存するのかを決めます。

> [action]
In `DisplayNoteViewController`, update `prepare(for:sender:)` to account for saving existing notes:
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
We've added quite a bit of new code, let's break this down:
上のコードがやっていること:
>
1. We add a if-let new clause to our guard statement to check that the destination view controller of our segue is of type `ListNotesTableViewController`. This allows us to safely type case our segue's destination view controller and access it for each case of our switch-statement.
1. if-let文を加えて、セグエの目的地が`ListNotesTableViewController`タイプかを確認します。これでセグエの目的地のビューコントローラーが安全にtype castされて、Switch文を使うことができるようになります。
1. We split our `save` case for our switch-statement into two. The first for if we're saving an existing note. We're able to know the note already exist because we check the `notes` property of our `DisplayNoteViewController` for a `non-nil` value. If the note exists, we update it's properties and reload the table view of our `ListNotesTableViewController`.
1. `save`を二つの条件に分けています。初めの方は、存在するノートを保存する場合です。`DisplayNoteViewController`の`notes`を確認することで、ノートが存在しているかを確認することができるのです。もしノートが存在していたら、プロパティを`ListNotesTableViewController`のテーブルビューをリロードします。
1. If the `note` property is `nil`, we know the user is creating a new note. In which case, we keep the previous code for creating a new note.
1. `note`プロパティが`nil`だったら、ユーザーは新しいノートを作っていることになります。このケースでは、前回作ったコードを再利用して新しいノートを作るようにします。
1. Notice that since we type cast our destination view controller in our guard-statement, we can use it directly without more downcasting.
1. guardステートメントで目的地のビューコントローラを型変換しているので、このビューコントローラをdowncastingすることなく使うことができます。

<!-- break -->

> [info]
In the case of saving an existing note, we don't have to pass the modified note back to our `ListNotesTableViewController`. This is because in Swift, class types are passed by reference.
存在するノートを保存するケースでは、変更されたノートを`ListNotesTableViewController`に移す必要はありません。これは、Swiftではclassタイプは参照によって移すことができるからです。
>
This means that both view controllers reference the same class instance. If the class instance is modified in one view controller, the other view controller will also access the same modified object.
つまり、どちらのビューコントローラーも同じクラスインスタンスを参照しているのです。もしクラスインスタンスがあるビューコントローラーで更新されたら、他のビューコントローラーからでもその更新されたオブジェクトにアクセスできるのです。
>
We do, however, need to make sure the table view knows to reload it's cells.

## Running the App
## アプリを実行する

In this section, we implemented the functionality to view and edit existing notes. Once again, let's make sure everything works!
このセクションでは、既に存在するノートを確認したり編集したりすることができるようになりました。全てちゃんと機能しているか確認しましょう！

> [action]
Build and run the app. Make sure you can create new notes and edit existing ones.
アプリをビルド・実行しましょう。新しいノートを作ったり、編集ができるかを確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p09_displaying_notes/editing_note_checkpoint.mp4)

In the next section, we'll look at how to implement the functionality for deleting existing notes.
次のセクションで、ノートの削除を実装します。
