---
title: "ビューコントローラーを作ろう"
slug: creating-view-controllers
---

_ノート_ アプリでは、テーブルビューコントローラーにノートのリストが表示されています。ノートをクリックすると、`DisplayNoteViewController`が _push_ されるように設定していきます。

まずは`DisplayNoteViewController`を設定する必要があります。新しいビューコントローラをストーリーボードに追加しましょう。

> [action]
`Main.storyboard` で、新しいビューコントローラーを追加します:
>
1. _Object Library_ からビューコントローラーをドラックしてストーリーボードに追加します。テーブルビューコントローラの右側に追加しましょう。
1. 新しく追加したビューコントーラーを選択します。
1. _Identity Inspector_ へ移動します。
1. _Class_ の中で、`DisplayNoteViewController`をカスタムクラスに設定します。
>
![New View Controller](assets/new_view_controller.png)

# 表示と編集

`DisplayNoteViewController`のUIを配置します。ノートのタイトルと本文が必要です。また、タイトルと本文は編集ができるようにする必要があります。

二つの`UIKit`を使います。テキストフィールドとテキストビューです。

チップ計算機のアプリでテキストフィールドを使いました。このアプリでは、`UITextField`を新たに使います。

また、`UITextView`を使います。これによって、複数行あるテキストを簡単に表示したり編集したりすることができます。

`DisplayNoteViewController`に加えていきましょう。

> [action]
`Main.storyboard`で、`DisplayNoteViewController`にテキストフィールドとテキストビューを追加します。
>
1. _Object Library_ からテキストフィールドをドラッグしてストーリーボードに追加します。テキストフィールドの上に少しスペースができるように配置しましょう。
テキストフィールドの下に、テキストビューを加えます。
>
![Display Note UI](assets/display_note_ui.png)

<!-- break -->

> [info]
テキストフィールドとビューコントローラの上の間にスペースがあるか再確認しましょう。後から必要になります。

## 制約を加える

_auto-layout(オートレイアウト)_ 制約を加えます。

> [action]
テキストフィールドに下の制約を加えます:
>
- (Text Field) Top Edge 10pts from _Safe Area_ Top Edge
- (Text Field) Leading Edge 15pts from _Safe Area_ Leading Edge
- (Text Field) Trailing Edge 15pts from _Safe Area_ Trailing Edge
>
![Text Field Constraints](assets/text_field_constraints.png)

テキストビューにも同じことを繰り返します。

> [action]
テキストビューに下の制約を加えます:
>
- (Text View) Top Edge 10pts from Text Field Top Edge
- (Text View) Leading Edge 15pts from _Safe Area_ Leading Edge
- (Text View) Trailing Edge 15pts from _Safe Area_ Trailing Edge
- (Text View) Bottom Edge 15pts from _Safe Area_ Bottom Edge
>
![Text View Constraints](assets/text_view_constraints.png)

これだけです！`DisplayNoteViewController`のサブビューに制約を加えました。制約に慣れてきましたね。

## チェックポイント

新しいビューコントローラーを加えました。また、カスタムクラスとSwiftソースファイルを繋げ、サブビューを加え、_auto-layout_ 制約も加えました。

実はまだ、新しいビューコントローラーは`UINavigationController`と繋がっていないので、アプリを開いて進行状況を確認することはまだできません。

ストーリーボードは下のようになっているはずです。

![Storyboard Checkpoint](assets/new_vc_checkpoint.png)

次に進みましょう！
