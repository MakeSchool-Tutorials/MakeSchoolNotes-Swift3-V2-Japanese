---
title: "カスタムテーブルビューを作る"
slug: custom-table-view-cells
---

In the previous section, we set up our table view controller to display some sample data. Next, we're going to update our `UITableViewCell` UI by building a custom table view cell.
前のセクションでは、テーブルビューコントローラーを設定して、サンプルデータを表示しました。次は、カスタムテーブルビューセルを作って`UITableViewCell`をアップデートしていきます。

When we're done, each cell will display's the note's title and last modified timestamp.
これで、それぞれのセルにタイトルと最後に編集した時間が表示されるようになります。

# Setting a Custom Row Height
# 列の高さを設定する

Before we create our custom cell, let's adjust the row height for each cell in our table view. This will make the cell height of each of the table view cells taller.
カスタムセルを作る前に、まずは列の高さを調整しましょう。列の高さを高くします。

> [info]
By default, each row, or cell, in a `UITableView` will have a row height of 44pts.
デフォルトでは、それぞれの列、もしくはセルの高さは44ptsに設定されています。

<!-- break -->

> [action]
In `Main.storyboard`, increase the table view's row height to the following:
`Main.storyboard`で、下のように列の高さを高くします:
>
1. Select the `UITableView` from the _Document Outline_ in your storyboard.
1. _Document Outline_ から`UITableView`を選択します。
1. Navigate to the _Size Inspector_ in the _Utilities area_.
1. _Utilities area_ の _Size Inspector_ を選択します。
1. Find the _Row Height_ field and change it's value from `44` to `60`.
1. _Row Height_ を探して、`44`から`60`へ値を変更します。
>
![Set Table View Row Height](assets/set_table_view_row_height.png)

Making the height of each cell taller gives us a little more room to layout our UI for each note table view cell.
列を高くすることで、テーブルビューセルにもっとスペースができました。

# Setting Our Cell UI
# セルのUIを設定する

With our heighten cells, let's layout our `UITableViewCell` UI.
`UITableViewCell` UI を作っていきましょう。

> [action]
Create a vertical stack view with two `UILabel` on your prototype cell:
Prototype Cellに二つの`UILabel`の入ったスタックビューを作成しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/create_vertical_stack_view.mp4)
>
Step-by-step:
ステップ:
>
1. Drag two labels from the _Object Library_ onto your blank prototype cell. Make sure to position both labels so that one is above the other.
1. _Object Library_ から`UILabel`をドラッグして、Prototype Cell に持っていきます。
1. Select both labels simultaneously. You can do this by selecting one label and shift-clicking on the other label.
1. 二つのラベルを同時に選択します。一つのラベルを選択した後、Shiftキーを押しながらもう一つのラベルを選択しましょう。
1. Click the _Embed In Stack_ button. If the labels are positioned correct, _Interface Builder_ should create a vertical stack view with the selected label.
1. _Embed In Stack_ をクリックしましょう。二つのラベルが正しく配置されていたら、_Interface Builder_ が vertical stack view (縦のスタックビュー)を作成したはずです。

Next, we'll set our _auto-layout_ constraints for our stack view.
_auto-layout_ の設定をしていきます。

> [action]
Set constraints for each edge of the stack view:
スタックビューの両端に制約(constraints)をつけます:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/set_stack_view_constraints.mp4)
>
Step-by-step:
ステップ:
>
1. Select the stack view using the _Document Outline_.
1. _Document Outline_ を使ってスタックビューを選択します。
1. Click the _Add New Constraints_ button and add the following constraints:
    - (Stack View) Top Edge 0pts from Super View (Content View) Top Edge
    - (Stack View) Leading Edge 15pts from Super View Leading Edge
    - (Stack View) Trailing Edge 15pts from Super View Trailing Edge
    - (Stack View) Bottom Edge 0pts from Super View Bottom Edge
1. _Add New Constraints_ をクリックして、下の制約を付け加えます:
    - (Stack View) Top Edge 0pts from Super View (Content View) Top Edge
    - (Stack View) Leading Edge 15pts from Super View Leading Edge
    - (Stack View) Trailing Edge 15pts from Super View Trailing Edge
    - (Stack View) Bottom Edge 0pts from Super View Bottom Edge

Finally, we'll need to add an equal height constraint to our labels to remove some constraint ambiguity.
最後に、ラベルに、高さを同じにする制約を加えます。

> [action]
Set an equal height constraint for both labels within the stack view:
スタックビューの中で、両方のラベルに同じ高さの制約を加えよう:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/set_equal_height_constraint.mp4)
>
Step-by-step:
ステップ:
>
1. Expand your stack view in your _Document Outline_.
1. _Document Outline_ のスタックビューの中身を開きます。
1. Select both labels within the stack view.
1. スタックビューの中のラベルを両方とも選択します。
1. With both labels selected, click the _Add New Constraints_ button.
1. ラベルが選択された状態で、_Add New Constraints_ ボタンをクリックします。
1. In the resulting popup, tick the _Equal Heights_ checkbox and click the _Add Constraints_ button.
1. ポップアップウィンドウの中から、 _Equal Heights_ をチェックし、_Add Constraints_ をクリックします。

Nice! Our cell's UI is coming along. So far you should have the following:
ナイス！セルUIが調整されました。

![Cell With Auto Layout](assets/cell_w_auto_layout.png)

## Adding Some Pizzazz
## スタイルを加える

With our cell's layout setup correctly, it's time to add some styling to our labels.
レイアウトの設定が終わったところで、ラベルにスタイルを加えましょう。

Let's start with the top label.
まずは上のラベルから始めます。

> [action]
In `Main.storyboard`, set the following attributes for the top (note title) label:
`Main.storyboard` の中で、上のラベルに下に書かれた属性を設定します:
>
![Note Title Attributes](assets/note_title_attributes.png)
>
**Attributes:**
**属性(Attributes):**
>
- _Text_: Change from `Label` to `Note Title`
- _Text_: `Label` から `Note Title`へ
- _Font_: Change from `System 17.0` to `System 18.0`
- _Font_: `System 17.0` から `System 18.0` へ
- _Font Color_: Change from `Black` to the hex color `#53A8D2`
- _Font Color_: `Black`から`#53A8D2` へ

Next, we'll change the attributes for the last modified timestamp label.
次に、タイムスタンプラベルの属性を変更します。

> [action]
In `Main.storyboard`, set the following attributes for the bottom (last modified timestamp) label:
`Main.storyboard` の中で、下のラベルに下に書かれた属性を設定します:
>
![Last Modified Timestamp Attributes](assets/last_modified_attributes.png)
>
**Attributes:**
>
- _Text_: Change from `Label` to `Last Modified Timestamp`
- _Text_: `Label` から `Last Modified Timestamp`へ
- _Font_: Change from `System 17.0` to `System 15.0`
- _Font_: `System 17.0` から `System 15.0` へ
- _Font Color_: Change from `Black` to the hex color `#67656C`
- _Font Color_: `Black`から`#67656C` へ

Next, we'll take a look at connecting our storyboard cell to it's corresponding Swift file.
次に、ストーリーボードのセルとSwiftファイルを繋げます。

# Setting The Cell Custom Class
# Cell Custom Class を設定する

Already included in your project, in the _Views_ group, is our custom table view cell subclass named `ListNotesTableViewCell`.
_Views_ グループの中に、`ListNotesTableViewCell`が既に入っています。

> [action]
Open `ListNotesTableViewCell.swift` from your _Project Navigator_.
_Project Navigator_　にある`ListNotesTableViewCell.swift`を開きましょう。
>
![Starting Cell Code](assets/starting_cell_code.png)

Right now, aside from the class definition, your source file should be empty.
クラスの定義をのぞいて、ファイルは空のはずです。

Similar to creating a custom subclass our table view controller, to implement a `UITableViewCell` subclass, we'll first need to set it's corresponding storyboard object's custom class.
これはテーブルビューコントーラーのサブクラスを作るときと似ていますが、`UITableViewCell`を実装するために、それに対応するストーリーボードのオブジェクトのカスタムクラスを設定する必要があります。

> [action]
In `Main.storyboard`, set your storyboard prototype cell's custom class:
`Main.storyboard`の中から、ストーリーボードのprototype cell のカスタムクラスを設定します:
>
![Set Cell Custom Class](assets/set_cell_custom_class.png)
>
Step-by-step:
ステップ:
>
1. Select your storyboard table view cell using the _Document Outline_.
1. _Document Outline_ を使って、ストーリーボードのテーブルビューセルを選択します。
1. Navigate to the _Identity Inspector_ in the _Utilities area_.
1. _Utilities area_ の _Identity Inspector_ を表示します。
1. In the _Custom Class_ section, set the _Class_ attribute to `ListNotesTableViewCell`. Auto-complete should help you select the correct class.
1. _Custom Class_ セクションで、_Class_ 属性を`ListNotesTableViewCell`にします。

Next, we'll need to create a way to access the title and last modified labels on our `ListNotesTableViewCell` class. Without a way to access both labels, we won't be able to set or modified them with code.
次に、`ListNotesTableViewCell`のタイトルと編集日時へアクセスできるようにする必要があります。アクセスができないと、コードを書いて設定や編集をすることができません。

Sound familiar? `IBOutlet` to the rescue!
`IBOutlet`の出番です！

# Creating Cell IBOutlets
# IBOutletを作ろう

We'll need to create a corresponding `IBOutlet` for each of our custom cell's labels. This will allow us to set and access each label programmatically.
二つのカスタムラベルに対して、`IBOutlet`を作る必要があります。これを行うことで、プログラムを書いてラベルを編集することができるようになります。

Let's start by creating an `IBOutlet` for our note title label.
タイトルラベルの`IBOutlet`を作ってみましょう。

> [action]
First start by opening the _Assistant Editor_:
_Assistant Editor_ を開きましょう:
>
1. Open `Main.storyboard` from your _Project Navigator_.
1. _Project Navigator_ から `Main.storyboard` を開きます。
1. Click on the _Show the Assistant Editor_ button in the tool bar. ![Open Assistant Editor](./images/assistant.png)
1. _Show the Assistant Editor_ をクリックします。![Open Assistant Editor](./images/assistant.png)
1. (Optionally) Hide both the _Navigator_ and _Utilities_ panes to create more screen space for the _Assistant Editor_. ![Hide Navigator and Utilities Panes](./images/hide.png)
1. (任意)　_Navigator_ と _Utilities_ を隠して　_Assistant Editor_　にスペースを作りましょう。![Hide Navigator and Utilities Panes](./images/hide.png)
1. (Troubleshooting) If you don't see the `ListNotesTableViewCell.swift` file show up in the second editor window, change the _Assistant Editor_ file by selecting `Manual > MakeSchoolNotes > MakeSchoolNotes > Views > ListNotesTableViewCell.swift` in the dropdown above. ![Troubleshooting Assistant Editor](./images/setup.png)
1. (トラブルシューティング) もし`ListNotesTableViewCell.swift`が見つからなかったら、`Manual > MakeSchoolNotes > MakeSchoolNotes > Views > ListNotesTableViewCell.swift`を選択して _Assistant Editor_ の表示を変更しましょう。![Troubleshooting Assistant Editor](./images/setup.png)

With our _Assistant Editor_ displaying storyboard file and `ListNotesTableViewCell.swift` source code side-by-side, we can create each label's `IBOutlet`.
_Assistant Editor_　に`ListNotesTableViewCell.swift`とストーリーボードが表示されたので、`IBOutlet`を作ることができます。

> [action]
Create a `IBOutlet` for each corresponding label:
`IBOutlet`を作ります:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/create_label_outlets.mp4)
>
For each label:
ステップ:
>
1. Select the label within the stack view using the _Document Outline_.
1. _Document Outline_ を使って、スタックビューを選択します。
1. Control-click from the label to the `ListNotesTableViewCell` class definition in the _Assistant Editor_.
1. ラベルをcontrol キーを押しながらクリックし、`ListNotesTableViewCell`へドラッグします。
1. Name the `IBOutlet` accordingly:
    - _Title Label_: `noteTitleLabel`
    - _Last Modified Timestamp_: `noteModificationTimeLabel`
1. `IBOutlet`の名前を下のように書き換えます:
    - _Title Label_: `noteTitleLabel`
    - _Last Modified Timestamp_: `noteModificationTimeLabel`

When you're done, you can exit the _Assistant Editor_ and switch back to the _Standard editor_.
_Assistant Editor_ を終了し、_Standard editor_ へ切り替えましょう

With our new `IBOutlet` connections, we're now able to access each label property in our `ListNotesTableViewCell`!
`IBOutlet`を作ったことによって、これで`ListNotesTableViewCell`からラベルにアクセスができるようになりました！

# Type Casting Our Cell
# Cellにキャスティングする

Before we move on, we need to make one additional change to table view data source code.
データソースコードを少し書き換えます。

> [action]
Open `ListNotesTableViewController.swift`. Take another look at our current implementation of `tableView(_:cellForRowAt:)`.
`ListNotesTableViewController.swift`を開き、`tableView(_:cellForRowAt:)`を見てみましょう。

You should see the following code:
下のコードが書かれています:

```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath)
    cell.textLabel?.text = "Cell Row: \(indexPath.row) Section: \(indexPath.section)"

    return cell
}
```

In the code above, our code uses `dequeueReusableCell(withIdentifier:for:)` to retrieve the appropriate table view cell. The string identifier we've provided as a parameter, matches the _Identifier_ in our storyboard's _Identifier_ attribute.
`dequeueReusableCell(withIdentifier:for:)`を使ってテーブルビューセルを取得しています。パラメターとして与えているstring identifierが、ストーリーボードの _Identifier_ と一致しています。

![Matching Cell Identifier](assets/matching_cell_identifier.png)

However, our code doesn't know that our storyboard cell has a custom class of `ListNotesTableViewCell`.
このコードは、ストーリーボードセルが`ListNotesTableViewCell`というカスタムクラスを持っていることを知りません。

![Cell Custom Class](assets/cell_custom_class.png)

Right now our code thinks cell has the default `UITableViewCell` type.
今の所このコードは、セルはデフォルトの`UITableViewCell`タイプを持っていると思っています。

> [action]
Option-click on the `cell` constraint. You should see the following:
`cell`の制約をoptionキーを押しながらクリックしましょう。
>
![Starting Cell Type](assets/starting_cell_type.png)
>
Since Swift is statically-typed, we can option-click on constants/variables and the compiler will show a popup with the current type of `cell`.
Swiftは静的型付けがされているので、制約や変数にoptionクリックをすることで、コンパイラーが現在の`cell`タイプをポップアップで表示してくれます。


 To change this, we'll need to use type casting to tell our code that the cell from the identifier has a custom class of `ListNotesTableViewCell`.
 コードにセルが`ListNotesTableViewCell`をカスタムクラスとして持っているということを教えてあげましょう。

> [action]
In `ListNotesTableViewController.swift`, downcast the cell in `tableView(_:cellForRowAt:)` to the correct custom class.
`ListNotesTableViewController.swift`にて、`tableView(_:cellForRowAt:)`のセルをダウンキャストして、正しいカスタムクラスへ設定しましょう。
>
```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath) as! ListNotesTableViewCell
>
    return cell
}
```
>
In the code above, we type cast our cell constant by using the `as` keyboard with the force unwrap optional `!`, followed the custom class we want type cast to, in this case, `ListNotesTableViewCell`. This is referred to as _downcasting_.
`as`とforce unwrap optional `!` を使って、セルの型変換を行なっています。_downcasting_　と呼ばれます。

<!-- break -->

> [info]
Be careful when downcasting! If the storyboard cell doesn't have it's custom class set to the type you're force casting to, this code will crash your app!
downcastingを行うときは注意しましょう！もしストーリーボードのセルがこのカスタムクラスをそもそも持っていなかったら、アプリがクラッシュします！

After type casting our cell, you can option-click on our cell constant to see that the type has changed from `UITableViewCell` to `ListNotesTableViewCell`.
セルの型変換を行なった後は、セルの制約をoptionクリックをすると`UITableViewCell`から`ListNotesTableViewCell`へ変わっているのが確認できるはずです。

![Type Cast Cell Type](assets/type_cast_cell_type.png)

Now that our cell has the correct type, we can also access the `ListNotesTableViewCell` properties. Let's set both labels that we created an `IBOutlet` for.
セルが正しい型になったので、`ListNotesTableViewCell`プロパティにアクセスができます。ラベルを設定しましょう。

> [action]
In `ListNotesTableViewController.swift`, update the method `tableView(_:cellForRowAt:)` to set each of the labels:
`ListNotesTableViewController.swift`で、`tableView(_:cellForRowAt:)`を変更して下記のように書き換えます。
>
```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath) as! ListNotesTableViewCell
    cell.noteTitleLabel.text = "note's title"
    cell.noteModificationTimeLabel.text = "note's modification time"
>
    return cell
}
```

**Important**: Verify that the code above was updated in your `ListNotesTableViewController.swift` and not accidentally added to your custom table view cell instead.
**重要**: 新たに関数を加えるのではなく、`ListNotesTableViewController.swift`の中に既にある関数を書き換えててください。

## Running The App
## アプリを実行する

We've created and implemented a custom table view cell with a new UI! Let's check our progress to make sure everything works as expected. Build and run your app.
新しいUIと、カスタムテーブルビューを実装しましたね！正しく実装されているか確認しましょう。

At this point, your app should look like the following:
この時点で、アプリはこのように見えているはずです。

![Custom Cell Checkpoint](assets/custom_cell_checkpoint.png)

If everything looks right, you're ready for the next section!
さあ、次へ進みましょう！
