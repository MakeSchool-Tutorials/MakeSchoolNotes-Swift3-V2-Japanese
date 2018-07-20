---
title: "CoreDataのヘルパーメソッドを実装する"
slug: CoreData-helpers
---

このセクションでは、_Core Data_ を使いやすくするために必要なヘルパーオブジェクトを作ります。このヘルパーオブジェクトは _Core Data_ コードを抽象化して、簡単にメソッドが使えるようにします。

ヘルパーオブジェクトは下のことができるようにします:

- 新しくノートを作る
- 既にあるノートを編集する
- ノートを削除する
- ノートを取得する

## 今までのデータモデルを削除する

_Core Data_ は自動的にそれ自身のデータモデルの`NSManagedObject`を生成するので、これまでに作ったデータモデルを取り除くことができます。

> [action]
`Note.swift`を削除します:
>
1. `Note.swift`を右クリックします。![Contextual Menu](assets/delete_context_menu.png)
1. _Delete_ を選択します。. ![Select Delete](assets/select_delete.png)
1. ポップアップウィンドウで、`Move to Trash`を選択します。![Delete Reference Popup](assets/delete_reference_popup.png)

## ノートエンティティを作成する

次に、古いデータモデルに替わる新しいデータモデルを生成します。そのために、`MakeSchoolNotes.xcdatamodeld`を使います。

> [action]
`MakeSchoolNotes.xcdatamodeld`を開いて、新しい`NSManagedObject`サブクラスをノートのデータに加えます。
>
1. _Editor_ の左下のに、_Add Entity_ ボタンが見えるはずです。このボタンをクリックしましょう。![Add Entity](assets/add_entity.png)
1. 左上に、全てのエンティティのアウトラインが見えるはずです、ダブルクリックをして、名前を`Note`に変えましょう。![Rename Entity](assets/rename_entity.png)
1. エンティティのアウトラインの右に、_Attributes_ というヘッダーが見えるはずです、下のプラス(+)ボタンを使って、下の３つのアトリビュートを加えましょう。![Note Attributes](assets/note_attributes.png)
  - _title_ タイプ `String`
  - _content_ タイプ `String`
  - _modificationTime_ タイプ `Date`
1. _Note_ エンティティを選択します。
1. _Note_ エンティティが選択された状態で、_Data Model inspector_　へ移動します。![Data Model Inspector](assets/data_model_inspector.png)
1. _Data Model inspector_ では、下のようにアトリビュートを設定しましょう。![Updated Data Model Inspector](assets/updated_data_model_inspector.png)
  - _Class_: 値が`Note`になっていることを確かめましょう。
  - _Module_: `Current Product Module`にセットしましょう。

## バグを直す

_Project Navigator_ でコードは見えませんが、`MakeSchoolNotes.xcdatamodeld` で _Note_ エンティティを作ることで、`Note`モデルを生成したことになります。

この _Core Data_ を使って生成された`Note`をモデルを使って、ノートの作成、保存、表示、編集、削除ができるようにします。

今アプリを起動すると、新しく作った`Note`モデルによって新しいエラーが出てしまいます。これは、`Note`クラスが、`Date?`型の`modificationTime`を持っているからです。

ささっと直しましょう。

> [action]
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
上のコードでは、シンプルにオプショナルチェイニングを使って、`modificationTime`を`Date?`型に変換しています。また、`modificationTime`が`nil`だったときにNil Coalescing Operatorを使ってデフォルトの値を設定しています。


# ヘルパーメソッドを作成する

> [action]
_Helpers_ グループに`CoreDataHelper.swift`ファイルを作成しましょう。

`CoreDataHelper.swift`ファイルに、ノートの追加、アップデート、取得、削除をするメソッドを作成します。

まずは _Core Data_ を使うために、インポートをする必要があります。

> [action]
`CoreDataHelper.swift`で、`UIKit`と`CoreData`をインポートしましょう:
>
```
import UIKit
import CoreData
```

次に、`static`を使ってコンスタントクラスを作り、managed object context へアクセスできるようにします。

> [action]
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
上のコードでは、computedクラス値を作っています。これはアプリのdelegate managed object contextを参照するものです。`NSManagedObjectContext`を参照して`NSManagedObject`オブジェクトを作成、編集、削除します。

## クラスメソッドを作成する

クラスメソッドはクラスで直接呼び出せるメソッドで、インスタンス化が必要ありません。Swiftでは、`static`を使うことでクラスメソッドを作ることができます。

例えば、`CoreDataHelper`オブジェクトに`doSomething()`と呼ばれるクラスメソッドを作成したい場合、下のコードのようにこのメソッドを呼びだすことができます。

```
CoreDataHelper.doSomething()
```

`doSomething()`をインスタンスメソッドとして作った場合、まず下のようなインスタンス化が必要です。

```
let helper = CoreDataHelper()
helper.doSomething()
```

## ノートの作成

staticメソッドを定義して新しいノートを作成し返すようにします。

> [action]
`CoreDataHelper`で、`Note`インスタンスを生成するstaticメソッドを作成します。
>
```
static func newNote() -> Note {
		let note = NSEntityDescription.insertNewObject(forEntityName: "Note", into: context) as! Note
>
		return note
}
```

簡単ですね！

## ノートを保存する

デフォルトの`NSManagedObjectContext`へノートを保存するstaticメソッドを定義します。

> [challenge]
`CoreDataHelper`に`saveNote`クラスメソッドを書いてみましょう。
>
困ったら、`newNote()`の例を見返してみましょう。

<!-- break -->

> [solution]
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

## ノートの削除

`Note`オブジェクトをパラメタにもつstaticメソッドを定義して、`NSManagedObjectContext`からノートを削除するようにしましょう。

> [challenge]
`CoreDataHelper`に`delete(note:)`というクラスメソッドを作ってみましょう。

<!-- break -->

> [solution]
下のようになるはずです。
>
```
static func delete(note: Note) {
    context.delete(note)
>
    saveNote()
}
```

## ノートを取得する

最後に、`NSManagedObjectContext`から全てのノートを取得するstaticメソッドを定義します。他のヘルパーメソッドと違い、これは`Note`オブジェクトの配列を返す必要があります。

> [action]
`CoreDataHelper`に`retrieveNotes()`というクラスメソッドを作り、`NSManagedObjectContext`から存在するノートを取得するようにしましょう。

<!-- break -->

> [solution]
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

# おわりに

たくさんのヘルパーメソッドを作りましたね！次は、`CoreDataHelper`を使ってデータの永続性を実装しましょう！
