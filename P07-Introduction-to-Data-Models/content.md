---
title: "データモデル入門"
slug: intro-data-models
---

Up to this point, we've set up our app UI and navigation between our two view controllers. In this section, we'll create a _data model_ to represent and store our note data.
この時点で、アプリのUIと２つのビューコントローラーのナビゲーションを実装しました。このセクションでは、_データモデル_ を作って、データが保存されるようにします。

As you continue to build apps, you'll need a way to represent objects to store data. Common examples of these objects might include a person, an animal, or even a note!
アプリを作っていく中で、データを保存するためのオブジェクトが必要になってくるでしょう。。例えば、人、動物、そしてノートのオブジェクトです！

For each of these objects, we want to store certain details (also referred to as data). In addition, each object may be able to perform certain actions.
それぞれのオブジェクトに対して、詳細のデータを保存したいですね。さらに、それぞれのオブジェクトはある特定のアクションを取れるようになるでしょう。

For the example of a person, we might want to store details about their name, birthdate and height. We could define a Swift class named `Person` with the following properties:
人の例だと、名前、誕生日、身長のデータを保存したいかもしれません。`Person`というクラスを作って下のようなプロパティを加えます。

```
class Person {

  // MARK: - Properties

  let name = "Daniel"
  let birthdate = Date()
  var height = 74

}
```

In addition, it's common for objects to perform certain actions. Following the example of a person, each person might be able to walk, run and jump.
さらに、特定のアクションを起こすことができるのはどのオブジェクトにも共通です。人の例では、歩く、走る、ジャンプする、などのアクションを実装することができます。

We can define a methods for our `Person` for the following actions:
`Person`クラスにメソッドを実装する方法:

```
class Person {

  // MARK: - Properties

  let name = "Daniel"
  let birthdate = Date()
  var height = 74

  // MARK: - Methods

  func walk() {
    // ...
  }

  func run() {
    // ...
  }

  func jump() {
    // ...
  }

}
```

For our _Notes_ app, we'll need to create a new object to represent and store our note data. But first, we'll need to create a new Swift file to hold our new class.
ノートアプリでは、ノートのデータを表示したり保存したりするオブジェクトが必要です。まず初めに、その新しいクラスを作るための新しいSwiftファイルを作りましょう。

# Creating A Swift File
# Swiftファイルを作成する

Creating a new Swift file is easy. In fact, you may have already done it before. Let's create a new Swift file named `Notes.swift`.
ファイルの作り方は簡単です。`Notes.swift`ファイルを作りましょう。

> [action]
In your _Project Navigator_, create a new, empty Swift file:
_Project Navigator_ に新しい、空のSwiftファイルを作りましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p07_introduction_to_data_models/create_notes_file.mp4)
>
Step-by-step:
ステップ:
>
1. Right click in your empty _Models_ group.
1. _Models_ グループを右クリックする
1. In the contextual menu popup, click _New File..._
1. _New File..._ をクリック
1. Make sure you select the Swift template. This will generate a new, empty Swift source file.
1. Swiftテンプレートを選択していることを確かめましょう。
1. Save your new file as `Notes.swift`.
1. `Notes.swift`という名前でファイルを保存しましょう。

# Create Our Data Model
# データモデルを作成する

Our note data model will need a title, it's last modified time and it's content.
このデータモデルはタイトル・編集日時と本文が必要です。

> [action]
In `Notes.swift`, create the following class:
`Notes.swift`で、下のクラスを作りましょう:
>
```
class Note {
>
    // MARK: - Properties
>
    var title = ""
    var content = ""
    var modificationTime = Date()
}
```

## Using Our New Model
## 新しいモデルを使う

Our table view controller currently uses hard-coded values to populate the labels for each table view cell. Let's update `ListNotesTableViewController` to use our new data model.
現在、テーブルビューコントローラーはコードを直接書き込んでラベルを表示させています。`ListNotesTableViewController`を書き換えて新しいデータモデルを使えるようにしましょう。

First, we'll need to create an array of notes for our `ListNotesTableViewController` to display.
`ListNotesTableViewController`に必要なノートの配列を作る必要があります。

> [action]
In `ListNotesTableViewController.swift`, add the following array to the top of your class:
`ListNotesTableViewController.swift`で、クラスの一番上に下の配列を加えましょう。
>
```
class ListNotesTableViewController: UITableViewController {
>
    // MARK: - Properties
>
    var notes = [Note]()
>        
    override func viewDidLoad() {
        super.viewDidLoad()
    }
>
    // ...
>
}
```
>
In the code above, we've added a empty `Notes` array to contain the user's notes.
上のコードによって、空の`Notes`配列をユーザーのノートに加えたことになります。

Next, we'll need to update our table view data source methods to display cells based on our new `notes` array.
次に、`notes`配列を元に、テーブルビューデータソースメソッドを書き換えます。

> [action]
In `ListNotesTableViewController.swift`, update both table view data source methods to the following:
`ListNotesTableViewController.swift`で、テーブルビューデータソースメソッドを下のように書き換えましょう。
>
```
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // 1
    return notes.count
}
>
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath) as! ListNotesTableViewCell
>    
    // 2
    let note = notes[indexPath.row]
    cell.noteTitleLabel.text = note.title
    // 3
    cell.noteModificationTimeLabel.text = note.modificationTime.convertToString()
>    
    return cell
}
```
>
In the code above:
上のコードでやっていること:
>
1. Dynamically return the number of notes in the `notes` array.
1. `notes`配列のノートの数を、動的に返す。
1. Retrieve the correct note based on the index path row and set the note cell's labels with the corresponding data.
1. index pathの列を元に正しいノートを取得し、それに対応するデータのラベルをセルに設定する。
1. We use the method `convertToString()` that was included in our project to convert our modification time from type `Date` to `String`.
1. `convertToString()`メソッドは、`Date`型から`String`型へ変換するためのメソッドです。

## Running the App
## アプリを実行する

Let's stop here to run and test our app. If you build and run, you should see the following:
アプリを実行して試してみましょう。下のようなアプリになっているはずです。

![Data Model Checkpoint](assets/data_model_checkpoint.png)

What happened to our notes?
ノートに何が起こったのでしょう？

That's right. If you remember, we're now using our `notes` array in our `ListNotesTableViewController` to display our notes. Since our array is initialized to be empty, our table view correctly doesn't display any cell.
これで正しいのです。`ListNotesTableViewController`で`notes`配列をを使ってノートを表示しています。今、配列が空なので、テーブルビューはまだ何も表示していないのです。

Next, we've implement the functionality for creating new notes with our _Create Note_ bar button item!
次は、_Create Note_　バーボタンを使ってノートを作成する機能を実装していきます！
