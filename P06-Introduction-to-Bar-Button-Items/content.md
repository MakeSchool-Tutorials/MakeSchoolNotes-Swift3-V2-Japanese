---
title: "バーボタンアイテム入門"
slug: intro-bar-button
---

In our _Notes_ app, a user will need to create, edit and/or discard notes. To do so, we'll need to introduce bar button items.
ノートアプリでは、ユーザーがノートを作成、編集、削除をできるようにしたいですね。そのために必要な、バーボタアイテムを紹介します。

`UIBarButtonItem` allows us to add additional buttons in our navigation bar.
`UIBarButtonItem`を使うことでナビゲーションバーにボタンを付け加えることができます。

![image showing bar button items](./images/barButtonItem.png)

To support the creating, editing, and discarding actions, we'll new to implement 3 new bar button items.
３つのボタンが必要です。

First, in our `ListNotesTableViewController`, we'll need to add a **create** bar button item for creating new notes.
まず、`ListNotesTableViewController`の中に、**create** バーボタンアイテムが必要です。

Next, in our `DisplayNoteViewController`, we'll add a **save** bar button item for editing existing notes and a **cancel** button for either discarding unintended changes or _popping_ the view controller off the navigation stack.
`DisplayNoteViewController`の中に、**save** ボタンと、**cancel** ボタンが必要です。

## Navigation Items
## ナビゲーションアイテム

To add `UIBarButtonItem` to a navigation bar in storyboard, the navigation bar must first have a _Navigation Item_.
`UIBarButtonItem`をナビゲーションバーに追加するために、ナビゲーションバーは _Navigation Item_ を
持っている必要があります。

If you open `Main.storyboard`, you can see that our `ListNotesTableViewController` has a _Navigation Item_ in the _Document Outline_.
`Main.storyboard` を開くと、`ListNotesTableViewController`が _Navigation Item_ を持っていることが確認できるはずです。

![Existing Navigation Item](assets/existing_navigation_item.png)

Conversely, if you look at our `DisplayNoteViewController` navigation bar, it doesn't have a _Navigation Item_ yet. Let's change that now.
`DisplayNoteViewController`のナビゲーションバーを見ると、_Navigation Item_ を持っていません。これを変更しましょう。

> [action]
In `Main.storyboard`, add a _Navigation Item_ to your `DisplayNoteViewController` navigation bar:
`DisplayNoteViewController`　ナビゲーションバーに、_Navigation Item_ を追加しましょう。
>
![Add Navigation Item](assets/add_nav_item.png)

Both of our storyboard view controllers should now have _Navigation Items_. Next, we'll add our create, save, and cancel `UIBarButtonItem` views.
これで、二つのストーリーボードビューコントローラーは _Navigation Items_ を持っています。次に、`UIBarButtonItem`ビューを追加していきます。

# Adding Bar Button Items
# バーボタンアイテムを追加する

Let's start by creating a new _Create Note_ bar button item in our `ListNotesTableViewController`.
`ListNotesTableViewController`に _Create Note_ バーを追加しましょう。

> [action]
In `Main.storyboard`, implement a new bar button item for your table view controller:
新しくバーボタンアイテムを追加します。
>
1. Drag a `UIBarButtonItem` from your _Object Library_ to the top right side of your `ListNotesTableViewController` navigation bar.
1. `UIBarButtonItem` を`ListNotesTableViewController`の右上に追加します。
1. Select the `UIBarButtonItem`.
1. `UIBarButtonItem` を選択します。
1. With the bar button item selected, navigate to the _Attributes Inspector_ in the _Utilities area_.
1. バーボタンアイテムが選択された状態で、_Attributes Inspector_ へ移動します。
1. Change the _System Item_ dropdown from `Custom` to `Add`.
1. _System Item_ を`Custom`から`Add`へ変更します。
>
![Add Create Bar Button Item](assets/add_create_bar_button_item.png)

One bar button item down, two more to go!
後二つです！

Repeat the same process above to add a save and cancel bar button item to your `DisplayNoteViewController`.
`DisplayNoteViewController`に、saveとcancelボタンを加えましょう。上でやったものと同じプロセスです。

> [action]
In `Main.storyboard`, implement both save and cancel buttons to your `DisplayNoteViewController`.
`DisplayNoteViewController`にsaveとcancelボタンを追加します。
>
![Save Cancel Bar Button Items](assets/save_cancel_bar_button_items.png)

We've successfully added 3 new bar button items. However, if you run the app right now, tapping on them do anything.
これで３つのバーボタンアイテムが追加されました。でも、今もしアプリを実行してボタンをタップしても、何も起こりません。

Next, we'll need to configure each bar button item so that tapping on it performs it's expected behavior.
これから、ボタンをタップしたら期待通りの動きが起こるように設定していきます。

# Bar Button Item Segues
# バーボタンアイテムセグエ

Each time the user taps on one of our bar button items, we want to either _push_ or _pop_ off of our navigation stack.
バーボタンをタップするたびに、_push_ もしくは _pop_ ボタンが起きるようにしたいですね。

In the case of our _Create Note_ button, when a user taps on the `UIBarButtonItem`, the `ListNotesTableViewController` should segue to the `DisplayNoteViewController`.
_Create Note_ ボタンの場合、`UIBarButtonItem` をタップしたら、`ListNotesTableViewController`が`DisplayNoteViewController`へ移動するように設定する必要があります。

Let's implement that now!
これを実装しましょう！

> [action]
In `Main.storyboard`, create a segue when the user taps the _Create Note_ bar button item and set it's identifier:
_Create Note_ バーボタンアイテムをタップしたときに実行されるセグエを作り、さらにIDを設定しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/create_add_segue_w_identifier.mp4)
>
Step-by-step:
ステップ:
>
1. Select the _Create Note_ bar button item in the navigation bar of your table view controller. It should look like a plus (+) button.
1. _Create Note_ ボタンを選択します。プラス(+)ボタンのように見えているはずです。
1. Hold control and click-drag from the _Create Note_ bar button item to the `DisplayNoteViewController`.
1. _Create Note_ バーボタンを選択したまま、controlを押して`DisplayNoteViewController`へドラッグします。
1. In the popup, select a _Action Segue_ of `Show` to create your new segue.
1. ポップアップウィンドウで、_Action Segue_ の `Show` を選択してセグエを作成します。
>
Next, let's set our new segue's identifier:
セグエIDを設定しましょう。
>
1. Select the new segue that appears between your `ListNotesTableViewController` and `DisplayNoteViewController`. When the correct segue is selected, you should see the _Create Note_ bar button item highlighted.
1. `ListNotesTableViewController`と`DisplayNoteViewController`の間の新しいセグエを選択します。正しいセグエが選択されていたら、_Create Note_ バーボタンアイテムがハイライトされているはずです。
1. With the segue selected, navigate to the _Attributes Inspector_ in the _Utilities area_.
1. _Attributes Inspector_ へ移動します。
1. Set the _Identifier_ field to `addNote`.
1. _Identifier_ 欄に`addNote`と入力します。

With our new segue identifier, we can update `prepare(for:sender:)` to notify us before the segue triggered by the _Create Note_ bar button item completes.
新しいIDを使って、`prepare(for:sender:)`をアップデートしましょう。

> [action]
Open `ListNotesTableViewController.swift` and update the method `prepare(for:sender:)` to the following:
`ListNotesTableViewController.swift`を開いて、`prepare(for:sender:)`をアップデートします。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>
    switch identifier {
    case "displayNote":
        print("note cell tapped")
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
In the code above, we update our if-statement to a switch-statement that checks for each segue identifier.
上のコードでは、if文ではなくswitch文を使っています。
>
For each identifier, the switch-statement outputs a corresponding print statement to the debug console.
それぞれのIDに対して、アウトプットを変えています。

Now, if we build and run our app, tapping on the _Create Note_ bar button item will segue to the `DisplayNoteViewController` and output a print statement to the debug console.
今アプリを実行すると、_Create Note_ をタップしたときに`DisplayNoteViewController`へ移動し、デバッグコンソールにメッセージが表示されているはずです。

Next, we'll look at how to implement segues for our `DisplayNoteViewController`.
次に、`DisplayNoteViewController`のセグエを実装していきます。

# Unwind Segues

Both the save and cancel bar button items will need to _pop_ the `DisplayNoteViewController` off the navigation stack.
Save と Cancelボタンは`DisplayNoteViewController`を _pop(手放す)_ 機能を持つ必要があります。

Because these segues are _pops_, instead of _pushes_, we'll need to implement a special type of segue known as a _unwind segue_.
このセグエは _pushe_ の代わりに _pops_ をするので、_unwind segue_ という方法を実装する必要があります。

_Unwind segues_ are used to _pop_ view controllers off the navigation stack. In other words, they're used to navigate back to previous view controllers on the navigation stack.
_Unwind segue_ はビューコントローラーを _pop_ する役割をします。言い換えると、これは前に表示されていたビューコントローラーに表示を戻す役割をするのです。

For instance, when a user taps the _Create Note_ bar button item, a segue is triggered and our navigation controller _pushes_ an instance of `DisplayNoteViewController` to the top of our navigation stack. Then, if we tap the cancel bar button item, we can use an _unwind segue_ to navigate back from the `DisplayNoteViewController` to the table view controller again.
例えば、_Create Note_ ボタンをタップしたとき、セグエを起動させて`DisplayNoteViewController` へプッシュします。そして、キャンセルボタンを押したとき、_unwind segue_ を使って`DisplayNoteViewController`からテーブルビューコントローラーに戻します。

![Notes Unwind Segue Example](./images/unwind-segue.png)

## Segue Memory Leaks
## セグエのメモリリーク

When navigating between multiple view controllers, be careful about creating memory leaks!
複数のビューコントローラを行き来するとき、メモリリークに気をつけましょう！

When triggering _show_ segues, a new instance of a view controller is _pushed_ onto the navigation stack. If we were to only use _show_ segues to navigate between view controllers, we would be continuously creating and adding new instances of view controllers onto the navigation stack. If unbounded, this could cause our app to run out of memory, potentially crashing our app!
_Show_ セグエを起動するとき、ビューコントローラの新しいインスタンスが _push_ されてナビゲーションスタックの上に積まれます。つまり、もし _show_ だけを使っていると、次々にビューコントローラのインスタンスが新たに生成されて行くのです。そのままだと、アプリがメモリ不足で止まってしまう恐れがあります！

In the image below, we only use segues to _push_ new instances of our two view controllers onto the navigation stack. After a few segues, our navigation stack has multiple instances of both our `ListNotesTableViewController` and `DisplayNoteViewController`.
下のイメージは、_push_ をするときだけセグエを使っています。何回かセグエを実行した後、ナビゲーションスタックは`ListNotesTableViewController`や`DisplayNoteViewController`のインスタンスを持っているはずです。

![image showing no unwind segues](./images/no-unwind-segue.png)

Bad! This is why we must be aware of our navigation stack and use _unwind segues_ to navigate back and forth between existing instances of view controllers.
ダメですね！これが _unwind segues_ を使わなければいけない理由です。

## Implementing Unwind Segues
## Unwind Segueを実装する

To make sure we don't keep adding new instances with _show_ segues, let's implement our _unwind segues_ for our `DisplayNoteViewController`.
`DisplayNoteViewController`に _unwind segues_ を実装しましょう。

Creating a _unwind segue_ requires two steps (in order):
_unwind segues_ を作る２つのステップ:

1. Create a special `@IBAction` method in the destination view controller that we're trying to unwind to. In our case, we'll put this method in our table view controller.
1. 戻ろうとしている目的地のビューコントローラに特別な`@IBAction`メソッドを作成します。今回のケースでは、テーブルビューコントローラーにこのメソッドを加えます。
1. Create a _unwind segue_ in our storyboard.
1. ストーリーボードに _unwind segue_ を作成します。

It's important to follow these steps in order. If we don't add our special `@IBAction` method in the destination view controller that we're unwinding to, we won't be able to create our _unwind segue_ in storyboard.
順番通りにステップを踏むことが大切です。`@IBAction`メソッドがない状態では、_unwind segue_ を追加することができません。

Let's start with step 1. We want to unwind from `DisplayNoteViewController` back to `ListNotesTableViewController`. For us, our destination view controller will be `ListNotesTableViewController`.
まずはステップ１から始めましょう。`DisplayNoteViewController`から`ListNotesTableViewController`へ"unwind"します。つまり、目的地は、`ListNotesTableViewController`です。

> [action]
In `ListNotesTableViewController`, add the following `@IBAction`:
`ListNotesTableViewController`に、下の`@IBAction`を追加しましょう:
>
```
@IBAction func unwindWithSegue(_ segue: UIStoryboardSegue) {
>
}
```
>
We don't need to add any code within our unwind `IBAction`. Xcode just needs to register the function definition for us to create our _unwind segue_ in storyboard.
`IBAction`の中には何もコードを加える必要はありません。_unwind segue_ を作るために必要なだけなのです。

Next, we'll follow step 2: create our _unwind segues_ for both cancel and save bar button items in storyboard. Let's start with our cancel bar button item.
ステップ２へ移ります。_unwind segues_ をcancelとsaveボタン両方に加えます。

> [action]
In `Main.storyboard`, create a _unwind segue_ for your cancel bar button item and give it a identifier:
`Main.storyboard`に、_unwind segue_ を加え、IDを与えます。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/create_cancel_segue_w_identifier.mp4)
>
Step-by-step:
ステップ:
>
1. Select your cancel bar button item.
1. キャンセルボタンを選択します。
1. Hold down control and click-drag from the cancel back button item to the orange exit icon on the top bar above your view controller.
1. controlを押しながらドラッグし、オレンジのexitアイコンへ引っ張ります。
1. In the popup, selected the `IBAction` you created in step 1.
1. ポップアップで、ステップ１で作成した`IBAction`を選択します。
>
Next, let's give our _unwind segue_ an identifier:
次に、_unwind segue_ にIDを加えます:
>
1. Select your _unwind segue_. You won't see it appear in storyboard, so you'll need to select it using your _Document Outline_.
1. _unwind segue_ を選択します。
1. With your segue selected, navigate to the _Attributes Inspector_ in the _Utilities area_.
1. セグエを選択したままで、_Attributes Inspector_ へ移動します。
1. Set the _Identifier_ field to `cancel`.
1. _Identifier_ 欄を`cancel`に設定します。

Next, we'll repeat the same steps for our save bar button item's _unwind segue_. We don't need to repeat step 1 since our `ListNotesTableViewController` already has a `IBAction` for _unwind segues_.
次に、同じステップをsaveボタンに対しても行います。ステップ１は必要ありません。

> [challenge]
Repeat step 2 for our `DisplayNoteViewController` save bar button item. Set the new _unwind segue_ to have an identifier of `save`.
`DisplayNoteViewController`のsaveボタンに対して、ステップ２を行いましょう。_unwind segue_ のIDを`save`に設定しましょう。
>
If you get stuck, refer back to the steps on how we previously created a _unwind segue_ for our cancel bar button item.
もし困ったら、いつでも前のセクションに戻ってcancelボタンの時はどうしたかを確認しましょう。

When you're done, we can add code to our `DisplayNoteViewController` to notify us before a _unwind segue_ completes. In addition, we can use our new identifiers to identify which segue is being triggered.
終わったら、`DisplayNoteViewController`にコードを加えて _unwind segue_ が実行される前に通知を送ります。加えて、設定したIDを使って、どちらのセグエが呼び出されたかを確認できるようにします。

> [action]
In `DisplayNoteViewController.swift`, add the following code to your view controller:
`DisplayNoteViewController.swift`　に下のコードを加えましょう。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    guard let identifier = segue.identifier else { return }
>    
    switch identifier {
    case "save":
        print("save bar button item tapped")
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
In the code above, we use `prepare(for:sender:)` to notify us if an _unwind segue_ from one of our bar button items is being triggered. The method `prepare(for:sender:)` is especially useful because we're able to prepare for navigating both back and forth between other view controllers.
上のコードは、`prepare(for:sender:)`を使って _unwind segue_  が起動したかどうかを知らせてくれます。`prepare(for:sender:)`メソッドはとても便利です。なぜなら、ビューコントローラが進む時も、戻る時もどちらにも使えるからです。

## Running the App
## アプリを実行する

In this section, we setup new segues for our bar button items. Let's test that each of our new bar button items work!
このセクションでは、新しいセグエを設定しました。きちんと動くか確認しましょう！

> [action]
Build and run the app. In the table view controller, click on the _Create Note_ bar button item. Next try the cancel and save buttons. Make sure each bar button item segues and outputs the correct print statement to the debug console.
アプリをビルド・実行しましょう。テーブルビューコントローラーで、_Create Note_ をクリックしましょう。次に、cancel と save ボタンを押してみましょう。どちらのボタンも正常な動きをしているか確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p06_introduction_to_bar_button_items/bar_button_item_segue_checkpoint.mp4)

Next, we'll look at how to start implementing our logic for our notes app!
次は、ノートアプリのロジックを実装していきます！
