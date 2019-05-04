# はじめに

僕のPCではデュアルブートしていて、両OSでデータが共有できるように共有パーティションを作ってあります。  

この共有パーティションはfat32というファイルシステムですが、このファイルシステム上では権限関連の概念がないらしいです。  

したがって、C/C++の環境構築をした際にエラーが起こりました。  
([この記事内](https://qiita.com/nsd24/items/805d0b53c67a1043e819)でも言及しました。)  

本記事では、このエラーの解決方法を説明していきます。  

# 前提環境

* Ubuntu 18.04.2 LTS
* コンパイルした後のファイルを実行できる環境([この記事内](https://qiita.com/nsd24/items/805d0b53c67a1043e819)参照)

# 具体的なエラー内容

[この記事内](https://qiita.com/nsd24/items/805d0b53c67a1043e819)で、コンパイルした後のファイルの実行ができませんでした。    

```
$ gcc -o hello hello.c && ./hello
bash: ./hello: 許可がありません
$ g++ -o hello hello.cpp && ./hello
bash: ./hello: 許可がありません
```

![1-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/850b05f9-5455-1a4e-b064-608580244a86.png)

# エラー原因

そもそも、fat32では実行権限が付与できないらしいことがわかりました。([参考サイト](https://forums.ubuntulinux.jp/viewtopic.php?id=15515))

実際にファイルの実行権限をGUIで確認してみると、以下のように付与してもリセットされてしまいました。  

![1-1.gif](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/a78c9fbb-13c6-16ec-f98e-73bc4300588a.gif)

# 解決方法

今回は、fat32じゃないシステム上に実行ファイルを作成し、それを実行するようにします。  

以下のコマンドで、ubuntuのシステムに実行ファイルだけ保存し、それを実行するようにしています。  

```
$ gcc -o /home/nsd/ドキュメント/hello hello.c && /home/nsd/ドキュメント/hello
$ g++ -o /home/nsd/ドキュメント/hello hello.cpp && /home/nsd/ドキュメント/hello
```

しかし、毎回これをやるのは非常にめんどくさいので、シェルスクリプトを書いて任意のコマンドを実行すると一気にコンパイルから実行までしてくれるようにやってみます。  

```C.sh
#!/bin/bash
gcc -o /home/nsd/ドキュメント/c-file $1 && /home/nsd/ドキュメント/c-file
```

```C++.sh
#!/bin/bash
g++ -o /home/nsd/ドキュメント/c++-file $1 && /home/nsd/ドキュメント/c++-file
```

パスは、fat32じゃないとこならどこでもいいです。  
そして、コマンドとして実行できるようにします。  

僕はC.shとC++.shを/home/nsd/ドキュメントの直下に置きました。  

そして、このファイルをおいた場所でターミナルを開き、以下のコマンドを実行します。  

```
$ sudo cp C.sh /bin/C-command
$ sudo cp C++.sh /bin/C++-command
```

このコマンドで、C-command ファイル名, C++-command ファイル名と打つことでどこからでもC/C++のファイルがコンパイルし実行できるようになりました！(コマンド名は何でもいいので好きにつけてもらってOKです。)

![1-2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/d2694fed-ef5b-c536-7099-ffd1f17be184.png)

実際に[以前書いた記事](https://qiita.com/nsd24/items/805d0b53c67a1043e819)でのHelloWorldのプログラムを実行してみます。  

```
$ C-command hello.c

$ C++-command hello.cpp
```

![1-3.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/c50af377-bdf7-3c7b-1997-1affe6924d72.png)

これで無事、fat32上にあるC/C++プログラムのコンパイル・実行が手軽にできるようになったと思います！

※もしこれで許可がないって出たら、/binに飛んで  

```
$ sudo chmod +x C-command
$ sudo chmod +x C++-command
```

で実行権限付与してからもっかいやってみてください。  

# まとめ

この記事では、fat32上にあるC/C++プログラムを、シェルスクリプトを使って簡単にコンパイル・実行できるようにしました。  
他のコンパイラ言語でも同様のことが起きると思うので、他の言語でもまた書いていこうと思います！  

環境構築の方法自体は[この記事](https://qiita.com/nsd24/items/805d0b53c67a1043e819)で書いたので、そちらの方をチェックお願いします！

# 参考文献

* [コンパイルして実行するシェルスクリプト C,C++](https://qiita.com/koshitake2m2/items/d188d70f549ae0138a75)
* [#!/bin/sh は ただのコメントじゃないよ！ Shebangだよ！](https://qiita.com/mohira/items/566ca75d704072bcb26f)