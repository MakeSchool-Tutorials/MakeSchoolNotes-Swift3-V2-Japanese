---
title: "CoreData入門"
slug: intro-coredata
---

In the current implementation of our _Notes_ app, we're able to create, edit and delete notes. However, currently our notes are all saved in-memory and not persisted between app launches. This means that if we terminate our app, all of notes will be deleted from memory.
今のノートアプリでは、ノートの作成、編集、削除が可能です。しかし、アプリを起動するたびにデータがリセットされてしまいます。

In this section, we'll introduce _Core Data_. A tool that can help us persist (save) our data between app launches. Finally, we'll be able to save notes and rest assured that they'll still be there later.
このセクションでは、_Core Data_ を学びます。このツールを使うと、アプリを起動してもデータを留めることができます。

# What's Core Data
# Core Data とは

_Core Data_ is an Apple framework for managing an object graph. That really just means that it's a way to create and manage special data models that come with a lot of built-in functionality.
_Core Data_ は、Applyのフレームワークで、オブジェクトグラフを管理するために使います。これは単に、様々なビルドイン機能を持った特別なデータモデルを作成して管理する方法という意味です。

The functionality that we care most about (at least for this tutorial), is the ability to persist our notes data to our device. _Core Data_ makes this super easy by abstracting the hard-core engineering behind an easy to use API that we can use to save data locally to our device (and retrieve it too!)
今回もっとも必要とする機能は、ノートのデータをデバイスに保存し続けるということです。_Core Data_ はこの機能をとても簡単に実装できるようにしてくれます。

In our _Notes_ app, we want to make sure each of our user's notes are saved with _Core Data_. Whenever the app launches, we want to use _Core Data_ to retrieve and display the user's previously existing notes.
このノートアプリでは、それぞれのノートが _Core Data_ に保存されるようにします。アプリをいくら再起動しても、_Core Data_ を使ってノートのデータを取得して表示できるようにしたいのです。

In the rest of this section, we'll go over the key concepts of _Core Data_. Please note that this section is purely informational. In other words, you won't add any of the code in this section in your _Notes_ app.
これからは、_Core Data_ のキーコンセプトを学んで行きます。このセクションではコードを書き換える作業は必要ありません。

So grab a bucket of popcorn and get ready for... knowledge!

## Importing Core Data
## コアデータをインポートする

To use the _Core Data_ framework in any Swift source file, you must first add the following import statement:
Swiftファイルで _Core Data_ を使うためには、プログラムの初めに下のコードを付け加える必要があります。

```
import CoreData
```

Usually this is added at the top of your source file, outside of your class definition, along with your other import statements.
これはソースファイルの一番初めに加えられることが多いです。

```
import UIKit
import CoreData

class ExampleClass {
	// ...
}
```

# Core Data Object Types
# コアデータのオブジェクトタイプ

You can use _Core Data_ to generate simple data models for you. You won't see the code in your project, but behind the scenes, _Core Data_ creates a new class definition that subclasses `NSManagedObject`.
_Core Data_ を使ってシンプルなデータモデルを生成することができます。コード上では見られませんが、裏側で、_Core Data_ は`NSManagedObject`をサブクラスに持つ新しいクラスの定義を生成します。

For instance, we could use _Core Data_ to automatically generate a data model of type `Person`. _Core Data_ would define the following class definition:
例えば、_Core Data_ を使うと、`Person`タイプのデータのモデルを自動的に生成することができます。_Core Data_ は下のようなクラス定義をしています。

```
class Person: NSManagedObject {

		@NSManaged var name = ""
		@NSManaged var age = 0
}
```

Notice that `Person` subclasses `NSManagedObject`. This is super important in order for our `Person` data model to interact with _Core Data_. Remember, we won't have to write or handle any of this code in our _Notes_ app. _Core Data_ will do it for us.
`Person`は`NSManagedObject`をサブクラス化しています。これは、`Person`のデータモデルが _Core Data_ とやり取りができるようにするために非常に大切なステップです。上のコードは _Core Data_ が自動的に生成してくれるもので、私たちが作ったり、編集したりする必要は全くありません。

## Initializing a Core Data Object
## コアデータオブジェクトを初期化する

Next, let's look at how to initialize a _Core Data_ defined data model. When creating an instance of our `Person` data model, we'll need to do the following:
_Core Data_ のデータモデルの初期化の方法を見ていきます。`Person`データモデルを作るとき、下のコードを書く必要があります。

```
var chris = NSEntityDescription.insertNewObject(forEntityName: "Person", into: context) as! Person
```

Woah! A lot of new stuff is happening. Let's break it down.
何やらたくさん起きていますね！ブレイクダウンしてみましょう。

1. We create a new `Person` object using the `NSEntityDescription` class method `insertNewObject(forEntityName:into:)`. All this does is create a new _Core Data_ object.
1. `NSEntityDescription`のメソッド`insertNewObject(forEntityName:into:)`を使って、`Person`オブジェクトを作ります。ここでやっていることは、_Core Data_ の作成です。
1. The parameters we pass to `insertNewObject(forEntityName:into:)` make sure _Core Data_ creates an instance of the correct class type and saves it to the right place.
1. `insertNewObject(forEntityName:into:)`へ渡したパラメタは、_Core Data_ が正しいクラスタイプのインスタンスを正しい場所に保存するように促しています。
1. Last, to use the properties of `Person`, we need to type cast the `NSManagedObject` to type `Person`.
1. `Person`のプロパティを使うために、`NSManagedObject`から`Person`へ型変換をしています。

## The NSManagedObjectContext
## NSManagedObjectContext

To save and retrieve data with _Core Data_, we need to use the `NSManagedObjectContext` object. In our _Notes_ app, we'll use the default `NSManagedObjectContext` accessed through our app delegate.
_Core Data_ でデータを保存したり取得したりするために、`NSManagedObjectContext`オブジェクトを使います。ノートアプリでは、デフォルトの`NSManagedObjectContext`を使います。

We can access our app's `NSManagedObjectContext` with the following code:
下のコードを使って、アプリの`NSManagedObjectContext`へアクセスできます:
```
// remember to import of UIKit and CoreData
let appDelegate = UIApplication.shared.delegate as! AppDelegate
let persistentContainer = appDelegate.persistentContainer
let context = persistentContainer.viewContext
```

## Saving The NSManagedObjectContext
## NSManagedObjectContextを保存する

Saving the `NSManagedObjectContext` saves any unsaved _Core Data_ objects locally on our device.
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
The `try` keyword means that calling `save()` on our `NSManagedObjectContext` may fail and _throw_ an error. In which case, we're able to use the catch block to specify what to do if `save()` fails.
`try` キーワードは、`save()`を`NSManagedObjectContext`で呼び出すことは失敗するかもしれず、その時はエラーを出します。このケースでは、`save()`が失敗したときに必要な命令を書くことができ亜mす。
>
If you're still interested in the do/try/catch paradigm, you can learn more with these
do/try/catchに興味がある人は、下のマテリアルを見て知識を深めましょう。
 [lecture slides](https://www.makeschool.com/tutorials/advanced-ios-development/error-handling-swift) or read the [official documentation](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/ErrorHandling.html).

## Saving a NSManagedObject
## NSManagedObjectを保存する

To save an object with _Core Data_, you can simply create a new instance of the `NSManagedObject` subclass and save the managed context.
_Core Data_ を使ってオブジェクトを保存するために、`NSManagedObject`サブクラスの新しいインスタンスを作って、managed contextを保存します。

For example, if we wanted to create and save a new `Person` object, we would write the following code:
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

In the code above, we've created a new `Person` instance and saved it locally using _Core Data_.
上のコードでは、新しい`Person`インスタンスを作り、_Core Data_ 使ってローカルに保存しています。

## Updating a NSManagedObject
## NSManagedObject をアップデートする

Updating a `NSManagedObject` subclass is just as easy. We just need to modify the object's property and save the `NSManagedObjectContext`.
`NSManagedObject`サブクラスをアップデートするのは簡単です。オブジェクトのプロパティを変更して、`NSManagedObjectContext`を保存するだけです。

We can change the `age` property of `meredithGrey`
`meredithGrey`の`age`プロパティを変更できます。

Let's look at how we would update the `age` property of the `meredithGrey` we created in the last step:
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

The code above, updates the `age` property of our existing `Person` instance and saved the change with _Core Data_.
上のコードは、存在している`Person`インスタンスの`age`プロパティを変更し、_Core Data_ で保存しています。

## Retrieving NSManagedObject(s)
## NSManagedObjectを取得する

To retrieve `NSManagedObject(s)`, we'll use a `NSFetchRequest`. A fetch request allows us to specify what type of object we want to retrieve with _Core Data_.
NSManagedObjectを取得するには、`NSFetchRequest`を使います。`NSFetchRequest`は、どのオブジェクトを _Core Data_ を使って取得したいかを明確にします。

For instance, we can retrieve all `Person` objects with the following code:
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

We use the method `fetch(_:)` on our `NSManagedObjectContext` instance to retrieve all objects of type `Person` saved to that `NSManagedObjectContext`.
`NSManagedObjectContext`インスタンスの`fetch(_:)`メソッドを使って全ての`Person`タイプのオブジェクトを`NSManagedObjectContext`へ保存しています。

## Deleting a NSManagedObject
## NSManagedObject を削除する

Last, let's learn how to delete a `NSManagedObject` subclass. `NSManagedObjectContext` has a `delete(_:)` method that takes a `NSManagedObject` as a parameters. After deleting the note, we must also save our `NSManagedObjectContext`.
`NSManagedObject`サブクラスを削除する方法を学びましょう。`NSManagedObjectContext`は`delete(_:)`メソッドを持っていて、`NSManagedObject`をパラメタとして持ちます。ノートを削除したあと、`NSManagedObjectContext`を保存する必要があります。

We can use this method to delete objects from a managed object context:
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

The code above has deleted our `meredithGrey` instance from our `NSManagedObjectContext`.
上のコードは`NSManagedObjectContext`から`meredithGrey`インスタンスを削除しています。

# Wrapping Up
# おわりに

In this section, we've learned a lot of... knowledge!
このセクションでは、たくさんの知識を学びましたね！

We've gone over how to add, modify, retrieve and delete `NSManagedObject` objects with _Core Data_.
_Core Data_ を使って`NSManagedObject`オブジェクトを加えたり、書き換えたり、取得したり、削除したりする方法を学びました。

Next, let's use our new-found knowledge of _Core Data_ to implement persistence for our notes.
次に、_Core Data_ の知識を実際にノートアプリに活かしましょう！

> [info]
"知識を所有しても、行動に移さなければ、貴金属をただ集めるのと変わらない。虚しく愚かなことである。活用の法則は普遍的である。それを侵すものは自然力との戦いに苦しむ。" - キバリオン
""
