---
title: "ノートを追加する"
slug: adding-notes
---

With our data model setup, we can begin adding our note functionality! But first, we'll need to do some initial setup.
データモデルをセットアップしたので、ノートを追加する機能を実装しましょう！まず初めに、初期設定が必要です。

# View Controller Setup
# View Controller セットアップ

We'll need to create an `IBOutlet` for our text field and text view respectively. We'll need these `IBOutlet` properties to reference our text field and text view in our code.
テキストフィールドとテキストフィールドに対して、`IBOutlet`を作リます。これらの`IBOutlet`は、テキストフィールドとテキストビューをコード上で参照するのに必要です。
Here we go again...

> [challenge]
Using the _Assistant Editor_, create an `IBOutlet` for the text field and text view in `DisplayNoteViewController`.
_Assistant Editor_ を使って、`DisplayNoteViewController`の中のテキストフィールドとテキストビューに`IBOutlet`を追加しましょう。
>
1. Name the text field's `IBOutlet` as `titleTextField`.
1. テキストフィールドの`IBOutlet`を`titleTextField`と名付けます。
1. Name the text view's `IBOutlet` as `contentTextView`と名付けます。
1. テキストビューの`IBOutlet`を`contentTextView`と名付けます。

Easy enough.


Next, let's use the `IBOutlet` connections we created to remove the initial filler text in our text view.
次に、`IBOutlet`を使って、テキストビューにデフォルトで表示されているテキストを削除しましょう。

## Removing Lorem Ipsum
## Lorem　Ipsumを削除する

Right now, if you tap the _Create Note_ bar button item, you'll see the following:
今、_Create Note_ をタップすると、下のように表示されるはずです。

![Filler Text](assets/filler_text.png)

As you can see, our text view contains a bunch of lorem ipsum which is filler text.
lorem ipsumのテキストで埋められているはずです。

That's not right... if a user wants to create a new note, the text view should be empty so that the user can begin taking notes.
これは正しくありませんね、、ユーザーが新しいノートを作るとき、テキストビューは空であるべきです。

Let's fix that now.
直しましょう。

> [action]
In `DisplayNoteViewController`, add the following code right below your `viewDidLoad` method:
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
The method `viewWillAppear(_:)` is a view controller lifecycle method that is called right before a view is about to appear on the screen.
`viewWillAppear(_:)`メソッドは、ビューコントローラのライフサイクルメソッドで、このビューがスクリーンに表示される直前に呼び出されるものです。
>
In the code above, we set the text property of both our text field and text view to an empty string. This removes the current lorem ipsum text that we saw previously from our text view.
上のコードで、テキストフィールドとテキストビューの両方のテキストプロパティをセットしています。これでlorem ipsumテキストがなくなり、空のテキストビューになりました。

# Create a New Note
# 新しいノートを作成する

With out setup complete, we're ready to create our first note.
これで、初めてのノートを作成する準備ができました。

> [action]
In `DisplayNoteViewController`, update the following code to `prepare(for:sender:)`:
`DisplayNoteViewController`で、`prepare(for:sender:)`に下のコードを書き換えましょう。
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
In the code above, we update our `save` switch-case to do the following:
上のコードでは、`save`のSwitch文を下のように書き換えています。
>
1. Initialize a new `Note` object.
1. `Note`オブジェクトを初期化する
1. Set the new note's title and content to the corresponding text field and text view text values. If either value is `nil`, we provide an empty string as the default value using the nil coalescing operation (??).
1. 新しいノートのタイトルと本文を、対応するテキストフィールドとテキストビューの値に書き換える。もしどちらかの値が`nil`だった場合、デフォルトの値として空のString値を与える。
1. Set the new note's last modified time to the current date and time.
1. 新しいノートの最終編集日時を現在の日時に設定する。

## Saving The Note
## ノートを保存する

We've created a new note, but it hasn't been saved yet. To save our new note, we want to add it to our `notes` array in our `ListNotesTableViewController`. To do so, we'll need to use segues to pass data between view controllers.
これで新しいノートが作成できましたが、まだ保存されていません。保存するために、このデータを`ListNotesTableViewController`の`notes`配列に追加する必要があります。セグエを使ってビューコントローラー同士でデータを受け渡しします。

> [action]
In `DisplayNoteViewController`, update the following code to `prepare(for:sender:)`:
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
        print("cancel bar button item tapped")
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
1. Each segue is a link between two view controllers and provides reference to both the source and destination view controllers. We can use the segue's `destination` property to reference the destination view controller. But, in order to access the destination view controller's properties, we need to type cast the destination view controller to type `ListNotesTableViewController`.
1. それぞれのセグエは二つのビューコントローラを繋ぎ、ソースと目的地のビューコントローラの情報を参照しています。しかし、目的地のビューコントローラーのプロパティにアクセスするためには、目的地のビューコントローラを`ListNotesTableViewController`タイプに変換する必要があります。
1. With a reference to `ListNotesTableViewController`, we're able to direct append our new note to it's `notes` array.
`ListNotesTableViewController`を参照することで、`notes` 配列に新しいノートを追加することができます。


## Reloading The Table View Data Source
## テーブルビューのデータソースを再読み込みする

Tapping the save bar button item will now add our newly created note instance to the `ListNotesTableViewController` note array. To finish up, we'll also need to notify our `UITableView` to reload it's data using the `UITableView` method `reloadData()`.
saveバーをタップすると、新しく作られたノートが`ListNotesTableViewController`のnote配列に追加されます。最後に、`UITableView`に追加されたことを知らせる必要があります。。`UITableView`のメソッドの`reloadData()`を使います。

> [info]
Table views don't automatically know when they should update their table view cell UI. When you want to update a table view by adding, modifying or deleting a cell, you need to explicitly tell the table view to reload it's data source.
テーブルビューは、テーブルビューセルUIを自動的にアップデートしてくれません。もしテーブルビューをアップデートしたい場合は、テーブルビューに変更を通知する必要があるのです。
>
Doing so will cause the table view to re-calculate it's table view cells using it's table view data source methods.
変更を通知すると、テーブルビューは再度テーブルビューセルを計算します。

But when should we tell the table view it's data?
でも、いつテーブルビューにそのデータを知らせればよいのでしょう？

We can use property observers! Property observers allow us to write code when a property has been changed.
Property observers(プロパティオブザーバー)の出番です。Property Observersを使うと、プロパティが変更されたときに実行されるコードを書くことができます。


> [action]
In `ListNotesTableViewController`, change your `notes` array property to the following:
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
In the code above, we add syntax to create a property observer for our `notes` array. As the name suggests, the `didSet` observes the `notes` property. If the property changes the code within the `didSet` block is executed.
`notes`配列にプロパティオブザーバーを作成するためにシンタックスを加えました。`didSet`は`notes`プロパティを見張っています。もしプロパティが変わったら、`didSet`のブロックの中身が実行させるのです。
>
For us, this is extremely useful for reloading the table view whenever a new note has been added to the `notes` array. Oh you fancy huh?
これはとても便利ですね。ノートが追加されるたびに、`notes`配列に追加されていきます。

## Running the App
## アプリを実行する

In this section, we implemented the functionality to add a new note to our `ListNotesTableViewController` notes array. We use `prepare(for:sender:)` to pass data between view controllers and a property observer to update our table view when a new note has been added.
`ListNotesTableViewController`のnotes配列にノートを追加する機能を実装しました。`prepare(for:sender:)`を使って、ビューコントローラ間でデータを受け渡し、プロパティオブザーバーを使って、新しいノートが作成されたときにテーブルビューがアップデートされるようにしました。

Let's test that everything works!
さあ、全てうまく機能しているか確かめましょう！

> [action]
Build and run the app. Create a new note using the _Create Note_ bar button item and then save it. Check that the new note shows up back in the table view controller.
アプリをビルドして実行します。_Create Note_ ボタンを押して新しいノートを作り、保存しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p08_adding_notes/create_note_checkpoint.mp4)

We've implemented the functionality for creating new notes, but what happens if we click on an existing note?
新しいノートを作ることができました。さて、作成されたノートをクリックしたら何が起きますか＞

![Empty Existing Note](assets/empty_existing_note.png)

We get a blank, empty note! Hmm... something's not right. Let's figure out how to display existing notes in the next section.
空のノートが出てきてしまいました！
次のセクションでこれを直しましょう。

> [info]
You also might be wondering, "Why are your notes are reset whenever you re-launch the app?"
”アプリを再起動するたびにノートがリセットされるのはなぜ？”と思っている人もいるかもしれません。
>
This is because we haven't set up persistence to save our note data yet. We'll get there soon, but in the mean time, our notes will be delete from memory each time the app is terminated.
これは、まだノートのデータを永続的に使う設定をしていないからです。この設定は、この先のチュートリアルの中で出てきます。
