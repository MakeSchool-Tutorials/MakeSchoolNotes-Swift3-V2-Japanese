---
title: "ナビゲーションコントローラー入門"
slug: intro-nav-controller
---

A `UINavigationController` is a view controller subclass that makes navigation between multiple view controllers easier. In addition to easier navigation, a navigation controller also has a lot of built-in functionality that we'll explore in this section.
`UINavigationController`はビューコントローラのサブクラスです。複数のビューコントローラーを行き来する機能を実装するのに必要です。また、それ以外にも様々な機能を持っています。

Let's look at Apple's Calendar app to get a better idea of how a navigation controller works.
Appleのカレンダーアプリを観察してナビゲーションコントローラの役割を見てみましょう。

Let's start with the calendar view of the entire year:
年間のカレンダーを表示します。

![calendar app year view](./images/year.png)

The top translucent bar at the top of the view controller is called the _navigation bar_. The navigation bar has a type of `UINavigationBar` and is automatically configured and setup when we use a navigation controller.
トップの半透明のバーをは、_navigation bar_ と呼ばれています。_navigation bar_ は　`UINavigationBar`タイプを持っています。

If we tap on a specific month, we can see that navigation controller has changed from displaying the year view controller to the month view controller. The month view controller has been _pushed_ on top of the year-view.
ある月をクリックしてみましょう。ナビゲーションコントローラーが年間から月間のビューに変わりましたね。月間のビューコントローラーが年間の上に　_push_　されたのです。

![calendar app month view](./images/month.png)

If we look at the navigation bar at the top, we can also see that there's a back button. Because the year view controller is still below the month view controller, the navigation bar automatically shows a back button for you to _pop_ back to the previous (year) view controller.
トップのナビゲーションバーを見ると、戻るボタンがあります。月間のビューコントローラーの下に年間のビューコントローラーがあるので、戻るボタンを押すと自動的に　_pop_　バックをして前に表示されていた（年間の）ビューコントローラーに戻ります。

As you can see, the `UINavigationController` provides a lot of convenient functionality for navigating back and forth between multiple view controllers.
`UINavigationController`は複数のビューコントローラを行き来するのにとても便利ですね。

From the month view controller, if we tap on a specific day, the navigation controller will _push_ the week view controller onto the current _navigation stack_.
月間から、ある日にちをタップすると、ナビゲーションコントローラーが現在の _navigation stack_ の上に週間のカレンダーを _push_ します。

![calendar app day view](./images/day.png)

So far, our navigation controller stack (in order from front to back) has the following view controllers:
ナビゲーションコントローラースタックは、下のビューコントローラーを持っていることになります。

1. Week View Controller
1. 週間のビューコントローラー
1. Month View Controller
1. 月間のビューコントローラー
1. Year View Controller
1. 年間のビューコントローラー

Each view controller is _pushed_ onto the navigation stack and covers the previous view controller. If at any time, we want to go back to the previous view controller, we can tap the back button to _pop_ the front-most view controller off the stack.
それぞれのビューコントローラーは _push_ されてナビゲーションスタックの上に配置され、それまでに表示していたビューコントローラーの上に重なります。これで、戻るボタンを押せば簡単にいつでも戻ることができるのです。

For example, if we hit the back button now, the week view controller would be popped off of the navigation stack and the next front-most view controller would be displayed. In this case, the month view controller.
例えば、今戻るボタンを押せば、この週間のビューコントローラーはナビゲーションスタックから消えて、次に配置されていたビューコントローラーが表示されます。（つまり月間のビューコントローラーです）

In our _Notes_ app, we'll have two different view controllers.
さて、私たちの _ノート_ アプリでは、二つのビューコントローラーがあります。

The first is our existing `ListNotesTableViewController`. This will always be the bottom most view controller on our navigation stack. It will display a list of all of our notes.
１つ目は、`ListNotesTableViewController`。これはいつもナビゲーションスタックの一番下に配置されます。ノートの全てのリストを表示します。

Next, we'll need another view controller to help us display individual notes. Lucky for you, our project already includes `DisplayNoteViewController` for this specific purpose.
次に、それぞれのノートを表示するビューコントローラーが必要です。`DisplayNoteViewController`がその役割を果たします。

When the user wants to create or view a note, we'll push an instance of `DisplayNoteViewController` on to our navigation stack. In this view controller, a user can create, edit, or view a note.
ユーザーがノートを見たり作ったりしたいとき、`DisplayNoteViewController`がナビゲーションスタックの上にプッシュされます。ここではノートを作ったり、編集したり、見ることができます。

But first, we'll need to implement our own `UINavigationController`.
まず、`UINavigationController`を実装していきます。

# Implementing Our Navigation Controller
# ナビゲーションコントローラを実装する

As you can see above, navigation controllers include a lot of functionality for navigating between view controllers. They're also super beginner-friendly and easy to set up.
ナビゲーションコントローラーはたくさんの機能を持っています。初心者に優しく、簡単に設定ができます。


> [action]
Open `Main.storyboard` and embed our existing table view controller in a navigation controller:
`Main.storyboard`を開き、ナビゲーションコントローラーを追加しましょう。
>
1. Select the existing table view controller.
1. 今使っているテーブルビューコントローラーを選択します。
1. With the table view controller selected, in the Xcode menu, select `Editor > Embed In > Navigation Controller`.
1. テーブルビューコントローラーを選択した状態で、`Editor > Embed In > Navigation Controller` を選択します。

>
![Embed In Navigation Controller](assets/embed_in_navigation_controller.png)

You should see your existing table view controller embed within the navigation controller:
ナビゲーションコントローラが埋め込まれました。

![Storyboard Navigation Controller](assets/storyboard_nav_controller.png)

## Adding A Navigation Bar Title
## タイトルを追加する

Another useful feature that `UINavigationController` provides, is the ability to give a title name to the top-most view controller that the navigation controller is displaying. This can be helpful for giving users a context for what they're looking at on the screen.
`UINavigationController`は、タイトルを追加することができます。

Our table view controller will display a list of notes that the user has created. Let's add a title to our navigation bar.
このテーブルビューコントローラーは、ノートのリストを表示しますね。これに合わせてタイトルを付け加えましょう。

> [action]
Set a title for our table view controller:
タイトルを付け加えよう:
>
1. Select the _Navigation Item_ by clicking on the navigation bar.
1. _Navigation Item_ を選択します。
1. Navigate to the _Attributes Inspector_ in the _Utilities area_.
1. _Attributes Inspector_ へ移動します。
1. In the _Title_ field, set the value from empty to `Notes`.
1. _Title_ に、`Notes`を書き込みます。
>
![Set Nav Bar Title](assets/set_nav_bar_title.png)

## Running the App
## アプリを実行する

We've embed our existing table view controller within a navigation bar. This will allow us to navigate between our `DisplayNoteViewController` later in this tutorial. Let's build and run to make sure everything still works!
ナビゲーションバーを埋め込みましたね。これでページを移動することができるようになりました。アプリを起動して正常に動くか確かめましょう！

At this point, your app should look like the following:
この時点で、アプリは下のように見えるはずです。

![Nav Bar Checkpoint](assets/nav_bar_checkpoint.png)

Ta-da! Our new navigation bar is looking fineee.
さあ、どんどん進みましょう。
