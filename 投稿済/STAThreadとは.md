はてなに投稿済み

# STAThreadとは - VB.NET

エントリポイントに記述されている`STAThread`についてのメモ

- エントリポイントには必要な記述
- シングルスレッドで動作していることを示す記述

```vb
Module Program
    <STAThread()> _
    Sub Main()
        Application.EnableVisualStyles()
        Application.SetCompatibleTextRenderingDefault(False)
        Application.Run(New Form1())
    End Sub
End Module
```

`STAThread`属性を指定すると、以下のような意味になるらしい。

> アプリケーションのCOMスレッドモデルがシングルスレッドアパートメント(STA: Single-Threaded Apartment)であることを示します

アプリケーションをシングルスレッドで実行するときに指定するらしい。

> STAは単一のスレッドでCOMを実行させるために必要

VB.NETでは、デフォルトでシングルスレッドだから、指定しなくてもいいけど、指定すると安全だよって感じらしい

> VB.NETの場合はデフォルトでSTAなので必要はありませんが、明示的に記述しておいた方がよいでしょう

エントリポイントにはSTAThreadをしていしないと警告が出るらしい。

> 実はVisual Studioのコード分析では、System.Windows.Forms名前空間を参照しており、MainメソッドにSTAThreadAttribute属性が付いていない場合、「Windows フォームのエントリ ポイントを STAThread でマークします」という警告が出ます。この警告の説明によると、STAThreadAttribute属性はWindowsフォームを使用するすべてのアプリケーションのエントリポイントに指定する必要があり、MTAはWindowsフォームでサポートされていないということです

# 参考文献

[STAThreadの意味は？ - .NET Tips (VB.NET,C#...)](https://dobon.net/vb/dotnet/form/stathread.html)