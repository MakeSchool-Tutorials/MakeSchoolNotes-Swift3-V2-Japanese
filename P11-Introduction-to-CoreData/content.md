---
title: "CoreData入門"
slug: intro-coredata
---

今のノートアプリでは、ノートの作成、編集、削除が可能です。しかし、アプリを起動するたびにデータがリセットされてしまいます。

このセクションでは、_Core Data_ を学びます。このツールを使うと、アプリを起動してもデータを留めることができます。

# Core Data とは

_Core Data_ は、Applyのフレームワークで、オブジェクトグラフを管理するために使います。これは単に、様々なビルドイン機能を持った特別なデータモデルを作成して管理する方法という意味です。

今回もっとも必要とする機能は、ノートのデータをデバイスに保存し続けるということです。_Core Data_ はこの機能をとても簡単に実装できるようにしてくれます。

このノートアプリでは、それぞれのノートが _Core Data_ に保存されるようにします。アプリをいくら再起動しても、_Core Data_ を使ってノートのデータを取得して表示できるようにしたいのです。

これからは、_Core Data_ のキーコンセプトを学んで行きます。このセクションではコードを書き換える作業は必要ありません。


## コアデータをインポートする

Swiftファイルで _Core Data_ を使うためには、プログラムの初めに下のコードを付け加える必要があります。

```
import CoreData
```

これはソースファイルの一番初めに加えられることが多いです。

```
import UIKit
import CoreData

class ExampleClass {
	// ...
}
```

# コアデータのオブジェクトタイプ

_Core Data_ を使ってシンプルなデータモデルを生成することができます。コード上では見られませんが、裏側で、_Core Data_ は`NSManagedObject`をサブクラスに持つ新しいクラスの定義を生成します。

例えば、_Core Data_ を使うと、`Person`タイプのデータのモデルを自動的に生成することができます。_Core Data_ は下のようなクラス定義をしています。

```
class Person: NSManagedObject {

		@NSManaged var name = ""
		@NSManaged var age = 0
}
```

`Person`は`NSManagedObject`をサブクラス化しています。これは、`Person`のデータモデルが _Core Data_ とやり取りができるようにするために非常に大切なステップです。上のコードは _Core Data_ が自動的に生成してくれるもので、私たちが作ったり、編集したりする必要は全くありません。

## コアデータオブジェクトを初期化する

_Core Data_ のデータモデルの初期化の方法を見ていきます。`Person`データモデルを作るとき、下のコードを書く必要があります。

```
var chris = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context) as! Person
```

何やらたくさん起きていますね！ブレイクダウンしてみましょう。

1. `NSEntityDescription`のメソッド`insertNewObject(forEntityName:into:)`を使って、`Person`オブジェクトを作ります。ここでやっていることは、_Core Data_ の作成です。
1. `insertNewObject(forEntityName:into:)`へ渡したパラメタは、_Core Data_ が正しいクラスタイプのインスタンスを正しい場所に保存するように促しています。
1. `Person`のプロパティを使うために、`NSManagedObject`から`Person`へ型変換をしています。

## NSManagedObjectContext

_Core Data_ でデータを保存したり取得したりするために、`NSManagedObjectContext`オブジェクトを使います。ノートアプリでは、デフォルトの`NSManagedObjectContext`を使います。

下のコードを使って、アプリの`NSManagedObjectContext`へアクセスできます:
```
// remember to import of UIKit and CoreData
let appDelegate = UIApplication.shared.delegate as! AppDelegate
let persistentContainer = appDelegate.persistentContainer
let context = persistentContainer.viewContext
```

## NSManagedObjectContextを保存する

`NSManagedObjectContext`を保存することで、保存されていない _Core Data_ オブジェクトをデバイスに保存することができます。

```
// reference to managed object context
let context: NSManagedObjectContext = ...

do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

> [info]
`try` キーワードは、`save()`を`NSManagedObjectContext`で呼び出すことは失敗するかもしれず、その時はエラーを出します。このケースでは、`save()`が失敗したときに必要な命令を書くことができます。
>
do/try/catchに興味がある人は、[講義スライド（英語）](https://www.makeschool.com/tutorials/advanced-ios-development/error-handling-swift) もしくは [公式ドキュメント（英語）](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html)を見て知識を深めましょう。

## NSManagedObjectを保存する

_Core Data_ を使ってオブジェクトを保存するために、`NSManagedObject`サブクラスの新しいインスタンスを作って、managed contextを保存します。

例えば、新しい`Person`オブジェクトを保存したいとき、下のようなコードを書きます:

```
// reference to managed object context
let context: NSManagedObjectContext = ...

// create a new NSManagedObject subclass instance
var meredithGrey = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context) as! Person

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

上のコードでは、新しい`Person`インスタンスを作り、_Core Data_ を使ってローカルに保存しています。

## NSManagedObject をアップデートする

`NSManagedObject`サブクラスをアップデートするのは簡単です。オブジェクトのプロパティを変更して、`NSManagedObjectContext`を保存するだけです。

`meredithGrey`の`age`プロパティを変更できます。

最後のステップで作った`meredithGrey`の`age`プロパティをアップデートする方法を見てみましょう。

```
let context: NSManagedObjectContext = ...
var meredithGrey: Person = ...

// update an existing NSManagedObject subclass instance
meredithGrey.age = 6

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

上のコードは、存在している`Person`インスタンスの`age`プロパティを変更し、_Core Data_ で保存しています。

## NSManagedObjectを取得する

NSManagedObjectを取得するには、`NSFetchRequest`を使います。`NSFetchRequest`は、どのオブジェクトを _Core Data_ を使って取得したいかを明確にします。

例えば、全ての`Person`オブジェクトを下のコードで取得できます。

```
// create a NSFetchRequest to retrieve all Person objects
let fetchRequest = NSFetchRequest<Person>(entityName: "Person")

// use the NSManagedObjectContext to execute the NSFetchRequest
do {
	let results = try context.fetch(fetchRequest)
} catch let error {
	print("Could not fetch \(error.localizedDescription)")
}
```

`NSManagedObjectContext`インスタンスの`fetch(_:)`メソッドを使って全ての`Person`タイプのオブジェクトを`NSManagedObjectContext`へ保存しています。

## NSManagedObject を削除する

`NSManagedObject`サブクラスを削除する方法を学びましょう。`NSManagedObjectContext`は`delete(_:)`メソッドを持っていて、`NSManagedObject`をパラメタとして持ちます。ノートを削除したあと、`NSManagedObjectContext`を保存する必要があります。

このメソッドを用いてオブジェクトを削除できます。

```
// reference to managed object context
let context: NSManagedObjectContext = ...

context.delete(meredithGrey)

// save the NSManagedObjectContext
do {
	try context.save()
} catch let error {
	print("Could not save \(error.localizedDescription)")
}
```

上のコードは`NSManagedObjectContext`から`meredithGrey`インスタンスを削除しています。

# おわりに

このセクションでは、たくさんの知識を学びましたね！

_Core Data_ を使って`NSManagedObject`オブジェクトを加えたり、書き換えたり、取得したり、削除したりする方法を学びました。

次に、_Core Data_ の知識を実際にノートアプリに活かしましょう！

> [info]
"知識を所有しても、行動に移さなければ、貴金属をただ集めるのと変わらない。虚しく愚かなことである。活用の法則は普遍的である。それを侵すものは自然力との戦いに苦しむ。" - キバリオン
""
