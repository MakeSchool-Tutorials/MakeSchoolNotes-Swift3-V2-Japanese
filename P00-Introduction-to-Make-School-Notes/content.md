---
title: "Make School Notesへようこそ！"
slug: welcome
---

This is the third tutorial of Make School's iOS app tutorial series.
これはiOSアプリチュートリアルシリーズの第三弾です。

In this tutorial, we'll focus on navigation and data persistence. First, we'll learn to use navigation controllers and segues to navigate between multiple view controllers. Additionally, we'll get started with _Core Data_ and saving data to our devices. Throughout the process, we'll build a notes app!
このチュートリアルでは、ナビゲーションとデータの永続性（えいぞくせい）に注目します。まず、ナビゲーションコントローラとセグエ
を使って複数のビューコントローラを行き来する方法を学びます。また、_コアデータ_ を使ってデバイスにデータを保存する方法も学びます。このプロセスの中で、ノートアプリを作りますよ！
"Are you ready for it?" - Taylor Swift

# Who Is This For?
# これは誰向けのチュートリアル？
iOS beginners who already have some experience with Xcode and building simple apps.
Xcodeを使って簡単なアプリを作ったことのある、iOSの初心者向けです。

## What You Should Already Know
## このチュートリアルの前に必要な知識
This tutorial builds on key concepts from the first two Make School tutorials.
このチュートリアルは初めの２つのアプリチュートリアルの中に出てきたコンセプトを基に構成されています。

下の方法を知っている必要があります:

- navigate your way around Xcode
- Xcodeに触り慣れている
- create `IBOutlet` and `IBAction` connections (on your own)
- `IBOutlet`と`IBAction`を自分で使うことができる
- be familiar with _Auto-Layout_
- _オートレイアウト_ に慣れている

If you're struggling with the concepts above, it's recommended to first complete the previous Make School tutorials.
もし不安がある場合は、初めのチュートリアルを見直してみましょう。

## Estimated Completion Time:
## このチュートリアルにかかる時間(目安):

4 時間

# What We're Building
# 何を作るのか

At the end of this tutorial, you'll have built your own _Notes_ app!
このチュートリアルを通して、_ノート_ アプリを作ります！

If you've been searching for an app to record all those ideas you've come up with in the shower, this one's for you! (But please first make sure your iPhone device is waterproof.)
もしたまたま、思いついたアプリのアイディアを記録するアプリを探していたら、このアプリがぴったりです！

![App Flow](assets/app_flow.png)

Your notes app will be able to:
このノートアプリでできること:

1. Create, edit and delete notes.
1. ノートの作成、編集、削除ができる
1. Give each note a title and body.
1. ノートにはタイトルと本文が追加できる
1. Save notes between app launches (data persistence.)
1. アプリを再起動してもノートは保存される（データの永続性)
1. View a scrollable list of existing notes.
1. ノートはスクロールをしてリストを見ることができる

# What You'll Learn
# 学ぶこと

By the end of this tutorial, you will:
このチュートリアルの最後には、

- learn to use `UITableView` and `UITableViewCell` to create scrollable views
- `UITableView`と`UITableViewCell`を使ってスクロールが可能な画面を作ることができる
- use `UINavigationController` and segues to navigate between multiple view controllers
- `UINavigationController`とセグエを使って複数のビューコントローラーを行き来させることができる
- how to pass data between different view controllers
- 違うビューコントローラ間でデータをやり取りすることができる
- persist data between app launches using _Core Data_
- _Core Data_ を使ってデータを永続的に保存することができる

> [info]
*Persisting data* is a fancy way of saying "saving data to your device". Normally, if you close your app, all your data is deleted. By using _Core Data_, we can save and retrieve our app's data between each app launch.
*データの永続性*　は、「データをデバイスに保存する」ことを格好良く言い換えたものです。普通、アプリを閉じると全てのデータは削除されます。_Core Data_ を使うことで、データを取り戻すことができるのです。

# Starter Project
# スタータープロジェクト

<!-- TODO: Needs to be updated to Swift4 -->
<!-- TODO: Swift４にアップデートされている必要があります -->

When you're ready, download the  and move onto the next section!

準備ができたら、[ここ](https://github.com/MakeSchool-Tutorials/MakeSchoolNotes-Swift-V2-Starter/archive/swift4-coredata.zip)からファイルをダウンロードしよう。

# If You Get Stuck
# もし困ったことがあったら

Getting stuck when coding (and debugging) is a natural part of the programming process. If you find yourself stuck on a problem or lost, pause for a moment and take a breath. Maybe take a walk. Then retrace your steps (in the tutorial, not the walk.) Make sure you've follow each step of the tutorial. It's easy to make typos or to accidentally skip over important steps.
コーディングをしていて課題に出会うことは、プログラミングのプロセスでは当たり前のことです。もし困ったり、迷うことがあったら、まずは深呼吸をして落ち着きましょう。少し立ち上がって歩いて見るのも良いでしょう。チュートリアルの全てのステップを完了しているか確認しましょう。一つでもスキップするとエラーが出ることがあります。

<!-- TODO: insert link to github repo "If you want to compare your code to the solution, you can find it here." -->
