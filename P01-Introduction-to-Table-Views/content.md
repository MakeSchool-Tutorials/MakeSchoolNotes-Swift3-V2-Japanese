---
title: "テーブルビュー入門"
slug: intro-table-view
---

In the apps we've built so far, we've learned how to build UI layouts using `UIKit` objects and _Auto-Layout_. But what if all of the content can't fit onto our device's screen at the same time?
ここまで作ったアプリの中で、`UIKit`や _Auto-Layout_ を使ってUIレイアウトを作るやり方を学びました。でももし、全てのコンテンツがどうしても画面に収まりきらなかったらどうしますか？

![Overflowing Content](assets/overflowing_content.png)

As in the example above, this is especially common for UI layouts involving feeds or lists of content.
上の例のように、フィードやコンテンツリストに特に共通しています。

![Table View Examples](assets/example_table_views.png)

Twitter uses this layout to a vertically scrollable feed of tweets. Likewise, Spotify uses the same UI layout to display a playlist of songs.
Twitterは縦にスクロールができるレイアウトを使っています。同様に、Spotifyも曲のリストにUIレイアウトを使っています。

To create this UI layout, we'll introduce a new `UIKit` object: `UITableView`. Table views provide a way for us to easily build vertical and horizontal scrolling content.
このUIレイアウトを作るために、新しい`UIKit`を紹介します: `UITableView` です。テーブルビューは縦や横にスクロールができるようにしてくれます。

A `UITableView` uses one or more `UITableViewCell` objects to create a scrollable list of cells. In most basic cases, each cell represents one item. For example, in our Twitter example, each tweet, or row, is a `UITableViewCell`.
`UITableView`の中には1つ以上の`UITableViewCell`が入っていて、スクロールができるリストを作ります。一番基本的な例は、`UITableViewCell`は一つのアイテムを示します。Twitterの例では、一つ一つのツイートが、`UITableViewCell`です。

![Table View Vs Cells](assets/table_view_vs_cells.png)

On the left, we have the _frame_ of our `UITableView`. Within the table view, we can see that each tweet, or row, is made up of a `UITableViewCell`.
左側には、`UITableView`の _frame_ が見えます。このテーブルビューの中に、`UITableViewCell`でできたツイートがみられますね。

> [info]
The `UITableView` class has a lot of depth. You can customize your table view to have headers, footers, multiple sections, etc. For the scope of this tutorial, we'll be covering the fundamentals. For more information on table views, check out this [explanation.](https://www.makeschool.com/tutorials/swift-concepts-explained/table-views)
実は、`UITableView`は多くのことができます。カスタマイズをしてヘッダーを加えたり、いくつかのセクションを加えたりすることができます。このチュートリアルでは、基本の使い方に縛りますが、もし興味がある人は[こちら](https://www.makeschool.com/tutorials/swift-concepts-explained/table-views)をチェックしてみましょう。

Next, let's look at how to create our own `UITableView`.
次に、`UITableView`の作り方を見てみましょう。

# Using an UITableViewController
# UITableViewController を使う

In our Notes app, we'll be using a subclass of `UIViewController` called `UITableViewController`. As the name might suggest, a `UITableViewController` is a `UIViewController` with a `UITableView` on it.
このノートアプリでは、`UIViewController` のサブクラスである、`UITableViewController`を使います。`UITableViewController`は`UIViewController`の上に`UITableView`が乗っかったものです。

Our starter project already contains our table view controller. Let's open it now.
スタータプロジェクトはtable view controllerが既に作成されています。開けてみましょう。

> [action]
Open your `Main.storyboard`, using your _Project Navigator_. ![Starting Storyboard](./images/open-main-storyboard.png)
_Project Navigator_ を使って、`Main.storyboard`を開きましょう。![Starting Storyboard](./images/open-main-storyboard.png)

Using `UITableViewController` are great for simple lists of content because they abstract a lot of boilerplate code that you'd usually need to implement a table view in a view controller.
`UITableViewController`はシンプルなリストに最適です。なぜなら、本来ならテーブルビューを作成するために自分で書かなければいけないコードを抽象化して使いやすくしてくれているからです。

Next, we need to pair our storyboard table view controller with it's Swift source file. If you recall from our previous tutorials, we can do this by setting our storyboard table view controller's custom class. Let's connect our storyboard's table view controller with our `ListNotesTableViewController` that's already included in our project.
次に、Swiftファイルと共に、ストーリーボードテーブルビューコントローラを繋げます。`ListNotesTableViewController`とストーリボードのテーブルビューコントローラを繋げましょう。

> [action]
Open `Main.storyboard` and set the custom class of the `UITableViewController`:
`Main.storyboard`を開き、`UITableViewController`カスタムクラスに設定しましょう。
>
1. Select your storyboard table view controller by selecting it in _Interface Builder_ or in the _Document Outline_.
1. _Interface Builder_ もしくは _Document Outline_ にある、ストーリーボードのテーブルビューコントーラを選択しましょう。
1. Open the _Identity Inspector_ in the _Utilities area_.
1. _Utilities area_ の _Identity Inspector_ を開きましょう。
1. In the _Custom Class_ section, set the _Class_ attribute to `ListNotesTableViewController`.
1. _Custom Class_ セクションの _Class_ を、`ListNotesTableViewController` に設定しましょう。
>
![Set Custom Class](./images/code-connection.png)

Now that we've set our custom subclass of our storyboard table view controller to `ListNotesTableViewController`, we've paired our storyboard table view controller with our Swift code. Let's begin adding code to `ListNotesTableViewController.swift` file.
ストーリーボードテーブルビューコントローラのサブクラスとして、`ListNotesTableViewController` を設定したので、Swiftコードとこのビューコントローラーがつながりました。`ListNotesTableViewController.swift`ファイルにコードを追加していきましょう。

# Displaying Table View Cells
# テーブルビューセルを表示する

With our `UITableViewController` subclass set, let's look at how to display a simple, unstyled table view cell.
`UITableViewController` サブクラスを使って、シンプルなテーブルビューセルを表示する方法をみてみましょう。

First, open your table view controller's source file.
まず、テーブルビューコントローラーのソースファイルを開きます。

> [action]
Open `ListNotesTableViewController.swift` in the _Controller_ group with your _Project Navigator_. ![Open Table View Controller](./images/ListNotesTableViewController.png)
_Controller_ グループ の中の `ListNotesTableViewController.swift` を開きましょう。

Even though our `ListNotesTableViewController` class is currently empty, this is where our table view controller related code will go.
`ListNotesTableViewController` のクラスが今のところ空ですが、ここにテーブルビューコントローラに関係するコードを加えていきます。

## Implementing Data Source Methods
## データソースメソッドを実装する

Hidden within the `UITableViewController` abstraction, are it's `UITableViewDataSource` methods. These methods provide the table view with the necessary information to display it's table view cells.
`UITableViewDataSource`は、`UITableViewController`の中に隠れています。このメソッドはテーブルビューセルを表示するのに非有ような情報をテーブルビューに与えているのです。

The two most important `UITableViewDataSource` methods are:
`UITableViewDataSource`の重要な二つのメソッド:

```
public func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int

public func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell
```

> [challenge]
From the method names above, can you guess what each method does?
上のコードが何をしているか当ててみましょう。

<!-- break -->

> [solution]
The method `tableView(_:numberOfRowsInSection:)` tells the `UITableView` how many `UITableViewCell` objects it should display. You might be wondering what the section parameter is referring to. As we briefly mentioned earlier, table views have a lot of built-in functionality, including the ability to have multiple sections. By default, each `UITableView` has 1 section with an index starting at `0`.
`tableView(_:numberOfRowsInSection:)`は`UITableView`に、いくつの`UITableViewCell`が表示されるべきかを教えてきます。numberOfRowsInSectionというパラメタが何を示しているのか気になるかもしれません。先ほど少し触れたように、テーブルビューは既にたくさんの機能を持っていて、複数のsectionを持つというのもその一機能です。デフォルトでは、`UITableView`は `0` から始まる１つのセクションを持っています。

>
The second method `tableView(_:cellForRowAt:)` returns the instance of the stylized `UITableViewCell` at the given `IndexPath`. A `IndexPath` is a simple data structure that contain's the cell's row and section indices.
`tableView(_:cellForRowAt:)`は、`IndexPath`の位置の`UITableViewCell`を生成して返します。`IndexPath`は、そのセルの列とセクションの番号を含んだシンプルなデータです。

To get a better idea of what these methods do, let's implement them in our table view controller.
よりよく理解するために、Table View Controllerに実装してみましょう。

> [action]
In `ListNotesTableViewController.swift`, add the following two methods:
`ListNotesTableViewController.swift`で、下記のメソッドを追加しましょう:
>
```
override func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
    // 1
    return 10
}
>
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    // 2
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath)
    cell.textLabel?.text = "Cell Row: \(indexPath.row) Section: \(indexPath.section)"
>
    return cell
}
```

By implementing the _data source_ method, we provide our table view controller's `UITableView` with information on what it should display. In the code above, we specify:

この _データソース_ を実装することで、テーブルビューコントローラの`UITableView`に何を表示するべきかを伝え
ることができました。上のコードでやっていること:

1. The table view should display 10 table view cells. This is hardcoded right now but eventually it'll reflect the number of notes the user has.
1. テーブルビューは10つのテーブルビューセルを表示する。
1. Return a table view cell (UIView subclass) instance. In addition, we configure the default `UITableViewCell` to display the cell's index path (row and section).
1. テーブルビューセルを返す。加えて、`UITableViewCell`はデフォルトでセルのindex pathを表示する設定にする。

<!--> [info]
There's a little more happening behind there scenes here!
>
1. First, our `UITableView` is using the _delegate pattern_ to communicate with the `UITableViewController`. As we've already mentioned, the table view needs to know the number of cells to display and cell instance for each index path. The _delegate pattern_ provides a specific way for one object to communicate with another. Usually, we'd have to set this up manually, but our `UITableViewController` does this for us behind the scenes. You can learn more about the _delegate pattern_ by clicking [here](https://www.makeschool.com/tutorials/swift-concepts-explained/delegates).
1. When we retrieve our `UITableViewCell` instance, we use the method `dequeueReusableCell(withIdentifier:for:)` to retrieve the instance of our `UITableViewCell` at a specific index path. This method hides some optimizations that determine if the table view will reuse a previously created cell or initialize a new cell instance. This prevents our app from creating a potentially infinite number of cells as we keep scrolling!
1. Finally, let's discuss the table view cell's identifier. The identifier is a unique string that allows us to connect our code to the correct `UITableViewCell` subclass. Before running our app, we'll need to set our identifier in our storyboard.
-->
## Setting the Cell Identifier
## Cell IDを設定する

To connect our table view cell, we need to set our cell identifier in storyboard. By setting our cell's identifier in storyboard, our code is able to identify the correct `UITableViewCell` subclass.
テーブルビューセルを繋ぐために、セルのIDを設定する必要があります。

> [action]
Open `Main.storyboard` and set the `UITableViewCell` identifier:
`Main.storyboard` を開いて、`UITableViewCell`を設定しよう:
>
1. Select the `UITableViewCell` instance in our table view controller by selecting the cell in the _Document Outline_.
1. _Document Outline_ から`UITableViewCell` を選択する![image illustrating location of table view cell](./images/tableViewCell.png)
1. Open the _Attributes Inspector_ in _Utilities area_. _Utilities area_ の _Attributes Inspector_ を開く![Open Attributes Inspector](./images/attributes-inspector.png)
1. Find the _Identifier_ field and set it's value to `listNotesTableViewCell`. _Identifier_ を探して、`listNotesTableViewCell`と入力する ![image illustrating location of identifier field](./images/identifier.png)

## Running The App
## アプリを実行する

With our cell identifier set, let's check our progress. Build and run your app in the simulator.
IDを設定し終わったら、一度アプリが動くか確認してみましょう。シミュレータを実行しましょう。

You should see the following:
下の画像のようなものが見えるはずです:

![Test Progress](assets/test_progress.png)

> [info]
Notice how each index path's row corresponds to the the index of each cell. In addition, since we're only using the default section, our section index is always 0.
index pathのrowがそれぞれのセルの番号と一致していることに気づきましたか？

Congrats on setting up your first table view controller! If something's gone wrong, try re-tracing each step to make sure you didn't miss any instructions. Two common mistakes are forgetting to set custom classes and cell identifiers.
初めのTable View Controllerを設定できましたね！もし上手くいかないようだったら、初めの方に戻って何か飛ばしていないか確認しましょう。Cell IDのタイプミスはよく見られる間違いです。

In the next section we will customize our table view cells so that we can display our note's title and modification time.
次のセクションでは、table view cellをカスタマイズして、ノートのタイトルと編集時間を表示させるようにします。
