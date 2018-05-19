---
title: "データモデル入門"
slug: intro-data-models
---

この時点で、アプリのUIと２つのビューコントローラーのナビゲーションを実装しました。このセクションでは、_データモデル_ を作って、データが保存されるようにします。

アプリを作っていく中で、データを保存するためのオブジェクトが必要になってくるでしょう。。例えば、人、動物、そしてノートのオブジェクトです！

それぞれのオブジェクトに対して、詳細のデータを保存したいですね。さらに、それぞれのオブジェクトはある特定のアクションを取れるようになるでしょう。

人の例だと、名前、誕生日、身長のデータを保存したいかもしれません。`Person`というクラスを作って下のようなプロパティを加えます。

```
class Person {

  // MARK: - Properties

  let name = "Daniel"
  let birthdate = Date()
  var height = 74

}
```

さらに、特定のアクションを起こすことができるのはどのオブジェクトにも共通です。人の例では、歩く、走る、ジャンプする、などのアクションを実装することができます。

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

ノートアプリでは、ノートのデータを表示したり保存したりするオブジェクトが必要です。まず初めに、その新しいクラスを作るための新しいSwiftファイルを作りましょう。

# Swiftファイルを作成する

ファイルの作り方は簡単です。`Notes.swift`ファイルを作りましょう。

> [action]
_Project Navigator_ に新しい、空のSwiftファイルを作りましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p07_introduction_to_data_models/create_notes_file.mp4)
>
ステップ:
>
1. _Models_ グループを右クリックする
1. _New File..._ をクリック
1. Swiftテンプレートを選択していることを確かめましょう。
1. `Notes.swift`という名前でファイルを保存しましょう。

# データモデルを作成する

このデータモデルはタイトル・編集日時と本文が必要です。

> [action]
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

## 新しいモデルを使う

現在、テーブルビューコントローラーはコードを直接書き込んでラベルを表示させています。`ListNotesTableViewController`を書き換えて新しいデータモデルを使えるようにしましょう。

`ListNotesTableViewController`に必要なノートの配列を作る必要があります。

> [action]
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
上のコードによって、空の`Notes`配列をユーザーのノートに加えたことになります。

次に、`notes`配列を元に、テーブルビューデータソースメソッドを書き換えます。

> [action]
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
上のコードでやっていること:
>
1. `notes`配列のノートの数を、動的に返す。
1. index pathの列を元に正しいノートを取得し、それに対応するデータのラベルをセルに設定する。
1. `convertToString()`メソッドは、`Date`型から`String`型へ変換するためのメソッドです。

## アプリを実行する

アプリを実行して試してみましょう。下のようなアプリになっているはずです。

![Data Model Checkpoint](assets/data_model_checkpoint.png)

ノートに何が起こったのでしょう？

これで正しいのです。`ListNotesTableViewController`で`notes`配列をを使ってノートを表示しています。今、配列が空なので、テーブルビューはまだ何も表示していないのです。

次は、_Create Note_　バーボタンを使ってノートを作成する機能を実装していきます！
