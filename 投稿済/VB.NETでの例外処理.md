はてなに投稿済み

# VB.NETの例外処理

例外処理について調べてみた時の走り書き！

> Javaには業務エラーを処理する検査例外があるので半ば強制的に例外をキャッチなりスローしなければいけませんが、.Net にはJavaの検査例外にあたるものがないので、業務エラーは原則戻り値で処理する設計になっています。従って、.Netでは余程のことが無い限り 処理中で例外をキャッチする必要はないし、することはお作法に反することになるのです。業務エラーは事前にチェックし、その戻り値でフローを制御する必要があるのです。

VB.NETではcatchしなくてもコンパイルエラーにならないため、別にTry-Catchで囲まなくてもよいのでは！？

もしくは、ログはいて、Throwするだけ？

~~集約例外ハンドラでログはく設定にしておけば、catchしなくてもよさそうだけど~~→集約例外ハンドラでログはけばいけたから、catchもしなくていい

- try-catchは、基本的に業務エラーに変換するときとかロールバックするとき以外は書かない！！
- try-finallyで十分。tryって書いたらcatchって書かないといけないことなんてない！！catchは書かなくていいの！！！

例１）ロールバックしたい場合

```vb
Try
    ' ロールバックしたい処理
Catch ex As SqlException
    ' ロールバック
    ' 必ずThrow
    Throw
Finally
    ' クローズ処理
```

例２）業務エラーに変換したい場合
```vb
Try
    ' 業務エラーに変換したい例外が発生する可能性のある処理
Catch ex As Exception
    If ex.Number = 2627 Then
        ' 業務エラーに変換
        MsgBox("重複エラー！")
    End If
Finally
    ' クローズ処理
```

これ以外にもたくさんあると思うけど、今はこれくらいしかわからない

## 検査例外

Javaにはあるけど、ほとんどの言語には存在しないもの

catchしないとコンパイルエラーになる例外

> 発生しうる例外の表明、発生しうる例外の保証

> 呼び出し側の責任ではない例外

## 非検査例外

catchしなくてもコンパイルエラーにならない例外

> 呼び出し側の責任の可能性がある例外

---

[検査例外と非検査例外 | hydroculのメモ](https://hydrocul.github.io/wiki/programming_languages_diff/exception/checked.html) によると以下のように記述されている

> ほとんどの言語では検査例外というのはなく、宣言なしに例外をスローすることができる

## リソースの開放はどうする？

`Using`構文を使うことで対処できる！？

コンパイラが Try-Finally に変換してくれるらしい

## 例外処理を1か所に記述する

集約例外ハンドラというものがあるらしい

[Scribbled Records - .Net Framework 例外処理の基本　集約例外ハンドラ（Windows フォーム）　C# でのサンプル](http://msyi303.blog130.fc2.com/blog-entry-13.html) 

### 参考文献

- [Javaの検査例外は、呼び出し側の責任でない異常系 - Qiita](https://qiita.com/yuba/items/d41290eca726559cd743)
- [ITとの生活：.Netの例外処理について: IT,車,不動産との生活](http://y373.cocolog-nifty.com/blog/2013/07/itnet-198f.html)
- [構造化例外処理 | ITツールの使い方](http://fksekiguchi.sakura.ne.jp/wp/it_basic_understanding/java/structuredexceptionhandling)
- [検査例外と非検査例外 | hydroculのメモ](https://hydrocul.github.io/wiki/programming_languages_diff/exception/checked.html)
- [検査例外再考 - Qiita](https://qiita.com/Kokudori/items/0fe9181d8eec8d933c98)
- [C# - 例外・ログメッセージ管理のプラクティス(100534)｜teratail](https://teratail.com/questions/100534)
- [Windowsフォーム開発で思ったこと - Life is Really Short, Have Your Life!!](http://aroundthedistance.hatenadiary.jp/entry/2011/10/13/195340)
- [Scribbled Records - .Net Framework 例外処理の基本　集約例外ハンドラ（Windows フォーム）　C# でのサンプル](http://msyi303.blog130.fc2.com/blog-entry-13.html)
- [Scribbled Records - .Net Framework 例外処理の基本　走り書き](http://msyi303.blog130.fc2.com/blog-entry-7.html)
- https://msdn.microsoft.com/ja-jp/library/ms229014(v=VS.100).aspx
- [例外処理。Try Catch、Try Finally、既定例外の基本的な動きと特性の理解。(UnhandledException,ThreadException) - tekkの日記 C#,VB.NET](http://d.hatena.ne.jp/tekk/touch/20120407/1333809539)