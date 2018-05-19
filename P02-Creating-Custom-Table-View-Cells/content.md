---
title: "カスタムテーブルビューを作る"
slug: custom-table-view-cells
---

前のセクションでは、テーブルビューコントローラーを設定して、サンプルデータを表示しました。次は、カスタムテーブルビューセルを作って`UITableViewCell`をアップデートしていきます。

これで、それぞれのセルにタイトルと最後に編集した時間が表示されるようになります。

# 列の高さを設定する

カスタムセルを作る前に、まずは列の高さを調整しましょう。列の高さを高くします。

> [info]
デフォルトでは、それぞれの列、もしくはセルの高さは44ptsに設定されています。

<!-- break -->

> [action]
`Main.storyboard`で、下のように列の高さを高くします:
>
1. _Document Outline_ から`UITableView`を選択します。
1. _Utilities area_ の _Size Inspector_ を選択します。
1. _Row Height_ を探して、`44`から`60`へ値を変更します。
>
![Set Table View Row Height](assets/set_table_view_row_height.png)

列を高くすることで、テーブルビューセルにもっとスペースができました。

# セルのUIを設定する

`UITableViewCell` UI を作っていきましょう。

> [action]
Prototype Cellに二つの`UILabel`の入ったスタックビューを作成しましょう。
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/create_vertical_stack_view.mp4)
>
ステップ:
>
1. _Object Library_ から`UILabel`をドラッグして、Prototype Cell に持っていきます。
1. 二つのラベルを同時に選択します。一つのラベルを選択した後、Shiftキーを押しながらもう一つのラベルを選択しましょう。
1. _Embed In Stack_ をクリックしましょう。二つのラベルが正しく配置されていたら、_Interface Builder_ が vertical stack view (縦のスタックビュー)を作成したはずです。

_auto-layout_ の設定をしていきます。

> [action]
スタックビューの両端に制約(constraints)をつけます:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/set_stack_view_constraints.mp4)
>
ステップ:
>
1. _Document Outline_ を使ってスタックビューを選択します。
1. _Add New Constraints_ をクリックして、下の制約を付け加えます:
    - (Stack View) Top Edge 0pts from Super View (Content View) Top Edge
    - (Stack View) Leading Edge 15pts from Super View Leading Edge
    - (Stack View) Trailing Edge 15pts from Super View Trailing Edge
    - (Stack View) Bottom Edge 0pts from Super View Bottom Edge

最後に、ラベルに、高さを同じにする制約を加えます。

> [action]
スタックビューの中で、両方のラベルに同じ高さの制約を加えよう:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/set_equal_height_constraint.mp4)
>
ステップ:
>
1. _Document Outline_ のスタックビューの中身を開きます。
1. スタックビューの中のラベルを両方とも選択します。
1. ラベルが選択された状態で、_Add New Constraints_ ボタンをクリックします。
1. ポップアップウィンドウの中から、 _Equal Heights_ をチェックし、_Add Constraints_ をクリックします。

ナイス！セルUIが調整されました。

![Cell With Auto Layout](assets/cell_w_auto_layout.png)

## スタイルを加える

レイアウトの設定が終わったところで、ラベルにスタイルを加えましょう。

まずは上のラベルから始めます。

> [action]
`Main.storyboard` の中で、上のラベルに下に書かれた属性を設定します:
>
![Note Title Attributes](assets/note_title_attributes.png)
>
**属性(Attributes):**
>
- _Text_: `Label` から `Note Title`へ
- _Font_: `System 17.0` から `System 18.0` へ
- _Font Color_: `Black`から`#53A8D2` へ

次に、タイムスタンプラベルの属性を変更します。

> [action]
`Main.storyboard` の中で、下のラベルに下に書かれた属性を設定します:
>
![Last Modified Timestamp Attributes](assets/last_modified_attributes.png)
>
**Attributes:**
>
- _Text_: `Label` から `Last Modified Timestamp`へ
- _Font_: `System 17.0` から `System 15.0` へ
- _Font Color_: `Black`から`#67656C` へ

次に、ストーリーボードのセルとSwiftファイルを繋げます。

# Cell Custom Class を設定する

_Views_ グループの中に、`ListNotesTableViewCell`が既に入っています。

> [action]
_Project Navigator_　にある`ListNotesTableViewCell.swift`を開きましょう。
>
![Starting Cell Code](assets/starting_cell_code.png)

クラスの定義をのぞいて、ファイルは空のはずです。

これはテーブルビューコントーラーのサブクラスを作るときと似ていますが、`UITableViewCell`を実装するために、それに対応するストーリーボードのオブジェクトのカスタムクラスを設定する必要があります。

> [action]
`Main.storyboard`の中から、ストーリーボードのprototype cell のカスタムクラスを設定します:
>
![Set Cell Custom Class](assets/set_cell_custom_class.png)
>
ステップ:
>
1. _Document Outline_ を使って、ストーリーボードのテーブルビューセルを選択します。
1. _Utilities area_ の _Identity Inspector_ を表示します。
1. _Custom Class_ セクションで、_Class_ 属性を`ListNotesTableViewCell`にします。

次に、`ListNotesTableViewCell`のタイトルと編集日時へアクセスできるようにする必要があります。アクセスができないと、コードを書いて設定や編集をすることができません。

`IBOutlet`の出番です！

# IBOutletを作ろう

二つのカスタムラベルに対して、`IBOutlet`を作る必要があります。これを行うことで、プログラムを書いてラベルを編集することができるようになります。

タイトルラベルの`IBOutlet`を作ってみましょう。

> [action]
_Assistant Editor_ を開きましょう:
>
1. _Project Navigator_ から `Main.storyboard` を開きます。
1. _Show the Assistant Editor_ をクリックします。![Open Assistant Editor](./images/assistant.png)
1. (任意)　_Navigator_ と _Utilities_ を隠して　_Assistant Editor_　にスペースを作りましょう。![Hide Navigator and Utilities Panes](./images/hide.png)
1. (トラブルシューティング) もし`ListNotesTableViewCell.swift`が見つからなかったら、`Manual > MakeSchoolNotes > MakeSchoolNotes > Views > ListNotesTableViewCell.swift`を選択して _Assistant Editor_ の表示を変更しましょう。![Troubleshooting Assistant Editor](./images/setup.png)

_Assistant Editor_　に`ListNotesTableViewCell.swift`とストーリーボードが表示されたので、`IBOutlet`を作ることができます。

> [action]
`IBOutlet`を作ります:
>
![ms-video](https://s3.amazonaws.com/mgwu-misc/Make+School+Notes/p02_creating_custom_table_view_cells/create_label_outlets.mp4)
>
ステップ:
>
1. _Document Outline_ を使って、スタックビューを選択します。
1. ラベルをcontrol キーを押しながらクリックし、`ListNotesTableViewCell`へドラッグします。
1. `IBOutlet`の名前を下のように書き換えます:
    - _Title Label_: `noteTitleLabel`
    - _Last Modified Timestamp_: `noteModificationTimeLabel`

_Assistant Editor_ を終了し、_Standard editor_ へ切り替えましょう

`IBOutlet`を作ったことによって、これで`ListNotesTableViewCell`からラベルにアクセスができるようになりました！

# Cellにキャスティングする

データソースコードを少し書き換えます。

> [action]
`ListNotesTableViewController.swift`を開き、`tableView(_:cellForRowAt:)`を見てみましょう。

下のコードが書かれています:

```
override func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
    let cell = tableView.dequeueReusableCell(withIdentifier: "listNotesTableViewCell", for: indexPath)
    cell.textLabel?.text = "Cell Row: \(indexPath.row) Section: \(indexPath.section)"

    return cell
}
```

`dequeueReusableCell(withIdentifier:for:)`を使ってテーブルビューセルを取得しています。パラメターとして与えているstring identifierが、ストーリーボードの _Identifier_ と一致しています。

![Matching Cell Identifier](assets/matching_cell_identifier.png)

このコードは、ストーリーボードセルが`ListNotesTableViewCell`というカスタムクラスを持っていることを知りません。

![Cell Custom Class](assets/cell_custom_class.png)

今の所このコードは、セルはデフォルトの`UITableViewCell`タイプを持っていると思っています。

> [action]
`cell`の制約をoptionキーを押しながらクリックしましょう。
>
![Starting Cell Type](assets/starting_cell_type.png)
>
Swiftは静的型付けがされているので、制約や変数にoptionクリックをすることで、コンパイラーが現在の`cell`タイプをポップアップで表示してくれます。


 コードにセルが`ListNotesTableViewCell`をカスタムクラスとして持っているということを教えてあげましょう。

> [action]
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
`as`とforce unwrap optional `!` を使って、セルの型変換を行なっています。_downcasting_　と呼ばれます。

<!-- break -->

> [info]
downcastingを行うときは注意しましょう！もしストーリーボードのセルがこのカスタムクラスをそもそも持っていなかったら、アプリがクラッシュします！

セルの型変換を行なった後は、セルの制約をoptionクリックをすると`UITableViewCell`から`ListNotesTableViewCell`へ変わっているのが確認できるはずです。

![Type Cast Cell Type](assets/type_cast_cell_type.png)

セルが正しい型になったので、`ListNotesTableViewCell`プロパティにアクセスができます。ラベルを設定しましょう。

> [action]
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

**重要**: 新たに関数を加えるのではなく、`ListNotesTableViewController.swift`の中に既にある関数を書き換えててください。

## アプリを実行する

新しいUIと、カスタムテーブルビューを実装しましたね！正しく実装されているか確認しましょう。

この時点で、アプリはこのように見えているはずです。

![Custom Cell Checkpoint](assets/custom_cell_checkpoint.png)

さあ、次へ進みましょう！
