---
title: "CoreDataのヘルパーメソッドを実装する"
slug: CoreData-helpers
---

In this section, we'll create a helper object to make interacting with _Core Data_ easier. Our helper object will abstract our _Core Data_ code into easy to use methods that we can use throughout the rest of our project.
このセクションでは、_Core Data_ を使いやすくするために必要なヘルパーオブジェクトを作ります。このヘルパーオブジェクトは _Core Data_ コードを抽象化して、簡単にメソッドが使えるようにします。

Our helper object will need to help us do the following:
ヘルパーオブジェクトは下のことができるようにします:

- create new notes
- 新しくノートを作る
- edit (modify) existing notes
- 既にあるノートを編集する
- delete notes
- ノートを削除する
- retrieve previously saved notes
- ノートを取得する

## Deleting Our Old Data Model
## 今までのデータモデルを削除する

Since _Core Data_ automatically generates it's own `NSManagedObject` version of a data model, we can get rid of the previous data model that we created.
_Core Data_ は自動的にそれ自身のデータモデルの`NSManagedObject`を生成するので、これまでに作ったデータモデルを取り除くことができます。

> [action]
Delete `Note.swift` from your Xcode project:
`Note.swift`を削除します:
>
1. `Note.swift`を右クリックします。![Contextual Menu](assets/delete_context_menu.png)
1. _Delete_ を選択します。. ![Select Delete](assets/select_delete.png)
1. ポップアップウィンドウで、`Move to Trash`を選択します。![Delete Reference Popup](assets/delete_reference_popup.png)

## Create a New Note Entity
## ノートエンティティを作成する

Next, we'll need to generate a new data model to replace our old one. To do this, we'll use the `MakeSchoolNotes.xcdatamodeld` provided in your project.
次に、古いデータモデルに替わる新しいデータモデルを生成します。そのために、`MakeSchoolNotes.xcdatamodeld`を使います。

> [action]
Open `MakeSchoolNotes.xcdatamodeld` with your _Project Navigator_, and create a new `NSManagedObject` subclass for your note data:
`MakeSchoolNotes.xcdatamodeld`を開いて、新しい`NSManagedObject`サブクラスをノートのデータに加えます。
>
1. On the bottom left of your _Editor_, you should see a button to _Add Entity_. Click this to begin creating your new data model.
1. _Editor_ の左下のに、_Add Entity_ ボタンが見えるはずです。このボタンをクリックしましょう。![Add Entity](assets/add_entity.png)
1. In the top left pane, you should see an outline of all of your entities. Double-click on your newly-created entity and change it's name to `Note`. ![Rename Entity](assets/rename_entity.png)
1. 左上に、全てのエンティティのアウトラインが見えるはずです、ダブルクリックをして、名前を`Note`に変えましょう。
1. On the right of the entities outline, you should see a section with the header _Attributes_. Use the plus (+) button at the bottom of the section to create the following 3 new attributes: ![Note Attributes](assets/note_attributes.png)
	You should have added the following attributes to your Note entity:
	- _title_ of type `String`
	- _content_ of type `String`
	- _modificationTime_ of type `Date`
1. エンティティのアウトラインの右に、_Attributes_ というヘッダーが見えるはずです、下のプラス(+)ボタンを使って、下の３つのアトリビュートを加えましょう。![Note Attributes](assets/note_attributes.png)
  - _title_ タイプ `String`
  - _content_ タイプ `String`
  - _modificationTime_ タイプ `Date`
1. Next, select your _Note_ entity in the outline.
1. _Note_ エンティティを選択します。
1. With your _Note_ entity selected, navigate to the _Data Model inspector_ in the _Utilities area_. ![Data Model Inspector](assets/data_model_inspector.png)
1. _Note_ エンティティが選択された状態で、_Data Model inspector_　へ移動します。![Data Model Inspector](assets/data_model_inspector.png)
1. In the _Data Model inspector_, set the following attributes: ![Updated Data Model Inspector](assets/updated_data_model_inspector.png)
	You should have set following fields in your _Data Model inspector_:
	- _Class_: Make sure the value is set as `Note`
	- _Module_: Set from empty to `Current Product Module`
1. _Data Model inspector_ では、下のようにアトリビュートを設定しましょう。![Updated Data Model Inspector](assets/updated_data_model_inspector.png)
  - _Class_: 値が`Note`になっていることを確かめましょう。
  - _Module_: `Current Product Module`にセットしましょう。

## Bug Fixing
## バグを直す

Although we can't see the code in our _Project Navigator_, by creating our _Note_ entity in `MakeSchoolNotes.xcdatamodeld`, Xcode has generated a `Note` model for us.
_Project Navigator_ でコードは見えませんが、`MakeSchoolNotes.xcdatamodeld` で _Note_ エンティティを作ることで、`Note`モデルを生成したことになります。

We'll be using this _Core Data_-generated `Note` model to create, save, display, edit and delete notes.
この _Core Data_ を使って生成された`Note`をモデルを使って、ノートの作成、保存、表示、編集、削除ができるようにします。

If you build and run the app right now, you'll notice there's a new error introduce by our new `Note` model. This is because our new `Note` class has a `modificationTime` property of type `Date?`.
今アプリを起動すると、新しく作った`Note`モデルによって新しいエラーが出てしまいます。これは、`Note`クラスが、`Date?`型の`modificationTime`を持っているからです。

Let's fix this real quick:
ささっと直しましょう。

> [action]
In `ListNotesTableViewController`, update your `tableView(_:cellForRowAt:)` with the following:
`ListNotesTableViewController`で、`tableView(_:cellForRowAt:)`を下のように書き換えましょう。
>
```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath) as! ListNotesTableViewCell
>
    let note = notes[indexPath.row]
    cell.noteTitleLabel.text = note.title
		// 1
    cell.noteModificationTimeLabel.text = note.modificationTime?.convertToString() ?? "unknown"
>
    return cell
}
```
>
In the code above, we simply use optional chaining to address our `modificationTime` being changed to type `Date?`. In addition, we use the nil-coalescing operator to provide a default value if `modificationTime` is `nil`.
上のコードでは、シンプルにオプショナルチェイニングを使って、`modificationTime`を`Date?`型に変換しています。また、`modificationTime`が`nil`だったときにNil Coalescing Operatorを使ってデフォルトの値を設定しています。

Next, let's move on to creating our helper methods!

# Creating Helper Methods
# ヘルパーメソッドを作成する

> [action]
Create a new, empty Swift file called `CoreDataHelper.swift` in the _Helpers_ group.
_Helpers_ グループに`CoreDataHelper.swift`ファイルを作成しましょう。

In our new `CoreDataHelper.swift` file, we'll create class methods that we can use to add, update, retrieve, and delete notes.
`CoreDataHelper.swift`ファイルに、ノートの追加、アップデート、取得、削除をするメソッドを作成します。

First, to use the _Core Data_ framework, we'll need to add the correct import statements at the top of the source file.
まずは _Core Data_ を使うために、インポートをする必要があります。

> [action]
In `CoreDataHelper.swift`, import the `UIKit` and `CoreData` frameworks:
`CoreDataHelper.swift`で、`UIKit`と`CoreData`をインポートしましょう:
>
```
import UIKit
import CoreData
```

Next, we'll create a class constant using the `static` keyword to access our app's managed object context:
次に、`static`を使ってコンスタントクラスを作り、managed object context へアクセスできるようにします。

> [action]
In `CoreDataHelper.swift`, add the following code:
`CoreDataHelper.swift`で、下のコードを加えましょう:
>
```
struct CoreDataHelper {
    static let context: NSManagedObjectContext = {
        guard let appDelegate = UIApplication.shared.delegate as? AppDelegate else {
            fatalError()
        }
>        
        let persistentContainer = appDelegate.persistentContainer
        let context = persistentContainer.viewContext
>        
        return context
    }()
}
```
>
In the code above, we create a computed class variable that gets a reference to our app delegate's managed object context. We can use our reference to our `NSManagedObjectContext` to create, edit and delete `NSManagedObject` objects.
上のコードでは、computedクラス値を作っています。これはアプリのdelegate managed object contextを参照するものです。`NSManagedObjectContext`を参照して`NSManagedObject`オブジェクトを作成、編集、削除します。

## Creating Class Methods
## クラスメソッドを作成する

Class methods are methods that can be called directly on the class, without having to instantiate an instance of the class first. In Swift, we can create class methods by placing the `static` keyword in front of our function definition.
クラスメソッドはクラスで直接呼び出せるメソッドで、インスタンス化が必要ありません。Swiftでは、`static`を使うことでクラスメソッドを作ることができます。

For example, if we want to create a class method called `doSomething()` in our `CoreDataHelper` object, we can call this method with the following code:
例えば、`CoreDataHelper`オブジェクトに`doSomething()`と呼ばれるクラスメソッドを作成したい場合、下のコードのようにこのメソッドを呼びだすことがでキアmす。

```
CoreDataHelper.doSomething()
```

If we created `doSomething()` as a instance method, we would need to first create an instance of the object first:
`doSomething()`をインスタンスメソッドとして作った場合、まず下のようなインスタンス化が必要です。

```
let helper = CoreDataHelper()
helper.doSomething()
```
Let's get started by creating our first class method for creating new notes!

## Creating Notes
## ノートの作成

We want to define a static method that creates a new note object, and then returns it.
staticメソッドを定義して新しいノートを作成し返すようにします。

> [action]
In `CoreDataHelper`, create a static method that creates a new `Note` instance:
`CoreDataHelper`で、`Note`インスタンスを生成するstaticメソッドを作成します。
>
```
static func newNote() -> Note {
		let note = NSEntityDescription.insertNewObject(forEntityName: "Note", into: context) as! Note
>
		return note
}
```

Easy enough! Try the next one on your own.
簡単ですね！

## Saving Notes
## ノートを保存する

We want to define a static method that saves that note to the default `NSManagedObjectContext`.
デフォルトの`NSManagedObjectContext`へノートを保存するstaticメソッドを定義します。

> [challenge]
Following what we learned creating class methods and saving `NSManagedObject` instances with _Core Data_, try creating a new class method named `saveNote` in `CoreDataHelper` to save notes.
`CoreDataHelper`に`saveNote`クラスメソッドを書いてみましょう。
>
If you get stuck, try looking back at our previous example of creating our `newNote()` class method or how to save data using _Core Data_ in the previous section.
困ったら、`newNote()`の例を見返してみましょう。

<!-- break -->

> [solution]
Your class method for saving notes should look like the following:
下のようになるはずです:
>
```
static func saveNote() {
    do {
        try context.save()
    } catch let error {
        print("Could not save \(error.localizedDescription)")
    }
}
```

## Deleting Notes
## ノートの削除

We want to define a static method takes a `Note` object a parameter and  deletes that note from our `NSManagedObjectContext`.
`Note`オブジェクトをパラメタにもつstaticメソッドを定義して、`NSManagedObjectContext`からノートを削除するようにしましょう。

> [challenge]
Try creating a new class method named `delete(note:)` in `CoreDataHelper` to delete a given note.
`CoreDataHelper`に`delete(note:)`というクラスメソッドを作ってみましょう。

<!-- break -->

> [solution]
Your class method for deleting a given note should look like the following:
下のようになるはずです。
>
```
static func delete(note: Note) {
    context.delete(note)
>
    saveNote()
}
```

## Retrieve Notes
## ノートを取得する

Last, we want to define a static method that retrieves all notes from our `NSManagedObjectContext`. Unlike the other helper methods, this one should return an array of `Note` objects.
最後に、`NSManagedObjectContext`から全てのノートを取得するstaticメソッドを定義します。他のヘルパーメソッドと違い、これは`Note`オブジェクトの配列を返す必要があります。

> [action]
Try creating a new class method named `retrieveNotes()` in `CoreDataHelper` the retrieves all previous existing notes from our `NSManagedObjectContext`.
`CoreDataHelper`に`retrieveNotes()`というクラスメソッドを作り、`NSManagedObjectContext`から存在するノートを取得するようにしましょう。

<!-- break -->

> [solution]
Your class method for retrieving a existing notes should look like the following:
下のようになるはずです:
>
```
static func retrieveNotes() -> [Note] {
    do {
        let fetchRequest = NSFetchRequest<Note>(entityName: "Note")
        let results = try context.fetch(fetchRequest)
>
        return results
    } catch let error {
        print("Could not fetch \(error.localizedDescription)")
>
        return []
    }
}
```

# Wrapping Up
# おわりに

We've created a lot of useful helper methods to help us easily integrate and use _Core Data_ in our project. Next, let's use our `CoreDataHelper` to finish implementing persistence!
たくさんのヘルパーメソッドを作りましたね！次は、`CoreDataHelper`を使ってデータの永続性を実装しましょう！
