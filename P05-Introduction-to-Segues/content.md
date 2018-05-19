---
title: "セグエ入門"
slug: intro-segues
---

With our new `DisplayNoteViewController` set up, we can implement the functionality for our `UINavigationController` to navigate between view controllers.
`DisplayNoteViewController`を設定して、`UINavigationController`の機能を実装することができるようになりました。

A navigation controller uses _segues_ (pronounced seg-way) to navigate from one view controller to another. A _segue_ allows us to _push_ and _pop_ view controllers on and off the navigation stack.
ナビゲ＝ションコントローラーは _segues(セグエ)_ を使って、ビューを移動します。_segues(セグエ)_ は 前のページで説明した _push_ と _pop_ を可能にします。

# Creating A Segue
# セグエを作る

In our _Notes_ app, when a user taps on a `ListNotesTableViewCell` in our table view controller, we want to trigger a segue to our `DisplayNoteViewController`.
_ノート_ アプリでは、`ListNotesTableViewCell`をタップすると、`DisplayNoteViewController`へ移動するようにしたいですね。

> [info]
Segues can be triggered programmatically and in storyboard with _Interface Builder_. This tutorial will focus on creating segues in storyboard.
セグエはプログラムを書いて実装することもできますが、今回は、ストーリーボードを使って実装します。

<!-- break -->

> [action]
In `Main.storyboard`, create a segue that's triggered when a user taps on a notes cell:
`Main.storyboard`で、セグエを作って、ユーザーがセルをタップしたら実行されるようにしましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p05_introduction_to_segues/cell_tap_segue.mp4)
>
Step-by-step:
ステップ:
>
1. Select the storyboard cell in your table view controller.
1. テーブルビューコントローラーで、ストーリーボードセルを選択します。
1. With your notes cell selected, control-click and drag to the `DisplayNoteViewController`.
1. 選択をしたままで、controlを押しながら`DisplayNoteViewController`へドラッグします。
1. In the popup, select a _Selection Segue_ of `Show` to create your new segue.
1. ポップアップが出てくるので、_Selection Segue_ の `Show` を選択します。

After creating your new segue, you'll see a segue arrow connecting the table view controller and `DisplayNoteViewController`. You can also see this segue in the _Document Outline_.
セグエを作った後、テーブルビューコントローラーとDisplayNoteViewControllerが繫がりました。

![New Segue](assets/new_segue.png)

You'll also notice that since we setup our text field with _auto-layout_ constraints using our _Safe Area_, when our `UINavigationController` added a navigation bar to our `DisplayNoteViewController`, our subviews re-positioned themselves automatically. This is because the _Safe Area_ now registers it's top edge as the bottom of the navigation bar. Neat!
`UINavigationController`を`DisplayNoteViewController`に追加した時、サブビューが自動的に再配置されたのに気づきましたか。_auto-layout_ を設定したためです。楽ですね！

## Adding A Segue Identifier
## セグエIDを追加する

Segue identifiers, similar to the table view cell identifiers we previously used, allow us to uniquely identify a storyboard segue in our Swift code.
セグエIDはテーブルビューセルIDと似ています。Swiftでコードを書くときにセグエを使うことができるようにするためです。

Let's add a segue identifier to our new segue.
セグエIDを追加しましょう。

> [action]
Add a segue identifier to the segue between our table view controller and display notes view controller:
セグエIDを追加します。
>
![Set Segue Identifier](assets/set_segue_identifier.png)
>
Step-by-step:
ステップ:
>
1. Select the segue by clicking on it.
1. セグえをクリックします。
1. With the segue selected, navigate to the _Attributes Inspector_ in the _Utilities area_.
1. _Attributes Inspector_ へ移動します。
1. Set the _Identifier_ field to `displayNote`.
1. _Identifier_ 欄 を`displayNote`と入力します

Now, we're able to reference this specific segue in our code using it's new identifier that we've just setup.

# Preparing For Segues
# セグエを使うための準備

Right before a segue is completed, we can use a built-in `UIViewController` method named `prepare(for:sender:)` to execute code just before the new view controller is _pushed_ on to the navigation stack.
`prepare(for:sender:)`と呼ばれる`UIViewController`のメソッドは、新しいビューコントローラーが _push_ される直前に実行されます。

> [info]
You should never called `prepare(for:sender:)` yourself. Each `UIViewController` will automatically notify you with the `prepare(for:sender:)` when segues are about to happen.
自分で`prepare(for:sender:)`を呼び出してはいけません。`prepare(for:sender:)`はセグエが実行される直前に自動的に呼ばれるからです。

This allows us to, as the name suggests, prepare for a segue. This will be especially useful for passing data from one view controller to the next.
これで、セグエを準備することができます。

Let's test it out!
さあ、実験してみましょう！

> [action]
Open `ListNotesTableViewController` with your _Project Navigator_, and add the following code below your table view data source methods:
`ListNotesTableViewController`を開き、下のコードを加えます。
>
```
override func prepare(for segue: UIStoryboardSegue, sender: Any?) {
    // 1
    guard let identifier = segue.identifier else { return }
>
    // 2
    if identifier == "displayNote" {
        print("Transitioning to the Display Note View Controller")
    }
}
```
>
In the code above:
上のコードでしていること
>
1. Check that the segue has a identifier.
1. セグエがIDを持っているか確認する
1. If so, check if the identifier is equal to `displayNote`. If yes, add a print statement to our debug console.
1. もしそうであれば、そのIDが`displayNote`と同じかを確認する。もしそうであれば、デバッグコンソールに状況を表示する。

## Running the App
## アプリを実行する

In this section, we setup a segue for when a user taps on a note cell in our `ListNotesTableViewController`. We've also implemented code that override our table view controller's `prepare(for:sender:)` and print to the debug console each time the segue is triggered.
このセクションでは、`ListNotesTableViewController`のセルをユーザーがタップしたときに呼び出されるセグエを設定しました。さらに`prepare(for:sender:)`を実装して、このセグエが呼び出されたときにデバッグコンソールに表示されるようにコードを加えました。

Let's test that our new segue works!
このセグエがきちんと動くか確認しましょう！

> [action]
Build and run the app. In the table view controller, click on a note cell. To _pop_ off the `DisplayNoteViewController` off the navigation stack, we can tap the back button. Navigate back and forth between the two view controllers and check that `prepare(for:sender:)` is printing to the debug console correctly.
アプリをビルド・実行しましょう。テーブルビューコントローラーで、ノートセルをタップしましょう。backボタンを押すと前の画面に戻ります。`prepare(for:sender:)`で実装したコードがデバッグコンソールに表示されているか確認しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p05_introduction_to_segues/segue_checkpoint.mp4)

If everything looks correct, let's _segue_ on to the next section and learn about how to add new bar button items to our navigation bar.
さあ、次のページへ進みましょう！
