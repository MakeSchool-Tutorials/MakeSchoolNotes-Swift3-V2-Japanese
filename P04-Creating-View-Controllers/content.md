---
title: "ビューコントローラーを作ろう"
slug: creating-view-controllers
---

In our _Notes_ app, when a user taps on a note cell in our table view controller, we want our navigation controller to _push_ our `DisplayNoteViewController` on to the top of the navigation stack.
_ノート_ アプリでは、テーブルビューコントローラーにノートのリストが表示されています。ノートをクリックすると、`DisplayNoteViewController`が _push_ されるように設定していきます。

In order for us to set this functionality up, first we'll need to set up our new `DisplayNoteViewController`. Let's get started by creating a new view controller object in our storyboard.
まずは`DisplayNoteViewController`を設定する必要があります。新しいビューコントローラをストーリーボードに追加しましょう。

> [action]
In `Main.storyboard`, create a new view controller:
`Main.storyboard` で、新しいビューコントローラーを追加します:
>
1. Drag a view controller from the _Object Library_ into your storyboard. Position it on the right-side of your table view controller.
1. _Object Library_ からビューコントローラーをドラックしてストーリーボードに追加します。テーブルビューコントローラの右側に追加しましょう。
1. Select your new view controller.
1. 新しく追加したビューコントーラーを選択します。
1. With the view controller selected, navigate to the _Identity Inspector_ in the _Utilities area_.
1. _Identity Inspector_ へ移動します。
1. In the _Class_ field, set the view controller's custom class to `DisplayNoteViewController`. Auto-complete should help you select the correct class.
1. _Class_ の中で、`DisplayNoteViewController`をカスタムクラスに設定します。
>
![New View Controller](assets/new_view_controller.png)

# Displaying and Editing
# 表示と編集

Next, we'll need to create our UI layout for `DisplayNoteViewController`. Our view controller will contain the note's title and a body for each note's content. The user should be able to edit the title and content of the note.
`DisplayNoteViewController`のUIを配置します。ノートのタイトルと本文が必要です。また、タイトルと本文は編集ができるようにする必要があります。

To provide this functionality, we'll use two `UIKit` objects: the text field and text view.
二つの`UIKit`を使います。テキストフィールドとテキストビューです。

We've already used a text field in our tip calculator app. In this app, we'll use the `UITextField` to edit and display the note's title.
チップ計算機のアプリでテキストフィールドを使いました。このアプリでは、`UITextField`を新たに使います。

As for the note's content, we'll need to introduce a new class we haven't used before: the `UITextView`. A text view provides an easy way to display and edit multiple lines of text.
また、`UITextView`を使います。これによって、複数行あるテキストを簡単に表示したり編集したりすることができます。

Let's add both to our `DisplayNoteViewController`.
`DisplayNoteViewController`に加えていきましょう。

> [action]
In `Main.storyboard`, add a text field and text view to the `DisplayNoteViewController`:
`Main.storyboard`で、`DisplayNoteViewController`にテキストフィールドとテキストビューを追加します。
>
1. Drag a text field from the _Object Library_ to the top of your storyboard view controller. Make sure there's a little space between the top of the view controller and your text field. This will be important for setting up our _auto-layout_ constraints next.
1. _Object Library_ からテキストフィールドをドラッグしてストーリーボードに追加します。テキストフィールドの上に少しスペースができるように配置しましょう。
1. Right under your text field, drag a text view from the _Object Library_ and position it's frame so that it roughly covers the remaining view.
テキストフィールドの下に、テキストビューを加えます。
>
![Display Note UI](assets/display_note_ui.png)

<!-- break -->

> [info]
Check that you've left some space between the top of the view controller and your text field. This will be important for setting up our _auto-layout_ constraints next.
テキストフィールドとビューコントローラの上の間にスペースがあるか再確認しましょう。後から必要になります。

## Adding Constraints
## 制約を加える

Let's add our _auto-layout_ constraints to both of our new views.
_auto-layout(オートレイアウト)_ 制約を加えます。

> [action]
In `Main.storyboard`, add the following constraints to the text field:
テキストフィールドに下の制約を加えます:
>
- (Text Field) Top Edge 10pts from _Safe Area_ Top Edge
- (Text Field) Leading Edge 15pts from _Safe Area_ Leading Edge
- (Text Field) Trailing Edge 15pts from _Safe Area_ Trailing Edge
>
![Text Field Constraints](assets/text_field_constraints.png)

Next, let's do the same for our text view.
テキストビューにも同じことを繰り返します。

> [action]
In `Main.storyboard`, add the following constraints to the text view:
テキストビューに下の制約を加えます:
>
- (Text View) Top Edge 10pts from Text Field Top Edge
- (Text View) Leading Edge 15pts from _Safe Area_ Leading Edge
- (Text View) Trailing Edge 15pts from _Safe Area_ Trailing Edge
- (Text View) Bottom Edge 15pts from _Safe Area_ Bottom Edge
>
![Text View Constraints](assets/text_view_constraints.png)

That's it! We've added our constraints for our `DisplayNoteViewController` subviews. You're getting pretty good at this...
これだけです！`DisplayNoteViewController`のサブビューに制約を加えました。制約に慣れてきましたね。

## Checkpoint
## チェックポイント

We've created a new view controller in storyboard, connected it's custom class to our Swift source file, added it's subviews and finally implemented each subview's _auto-layout_ constraints.
新しいビューコントローラーを加えました。また、カスタムクラスとSwiftソースファイルを繋げ、サブビューを加え、_auto-layout_ 制約も加えました。

Because our new view controller isn't yet connected to our `UINavigationController`, there's no way for us to check our progress other than comparing storyboards.
実はまだ、新しいビューコントローラーは`UINavigationController`と繋がっていないので、アプリを開いて進行状況を確認することはまだできません。

Your storyboard should now look like the following:
ストーリーボードは下のようになっているはずです。

![Storyboard Checkpoint](assets/new_vc_checkpoint.png)

We'll setup our navigation controller to access our `DisplayNoteViewController` using segues in the next section. Onwards!
次に進みましょう！
