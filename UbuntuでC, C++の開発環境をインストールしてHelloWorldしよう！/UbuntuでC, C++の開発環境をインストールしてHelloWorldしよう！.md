# はじめに

この記事では、  
1. UbuntuにC/C++言語の開発環境をインストールする
2. HelloWorldする  
の2つについて書いていきまーす

# 前提環境

* Ubuntu 18.04.2 LTS

# 1. UbuntuにC/C++言語の開発環境をインストールする

C/C++言語はコンパイラ言語であるため、コンパイルした後実行する必要があります。  
実は、Ubuntuにはデフォルトでコンパイラが入っています。  
ctrl+alt+t キーを押すことでターミナルが開くので、以下のコマンドを実行して確認してみましょう。  
gccはC用のコンパイラで、g++はC++用のコンパイラです。  

```
$ gcc --version
$ g++ --version
```

僕の環境ではgcc7.4.0、g++7.4.0のバージョンが入っていることがわかります。  

![1-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/b3b47b9c-41eb-e95c-d02b-78b734904916.png)

しかし、現在のUbuntuではコンパイルしたプログラムを実行することはできません。  
そこで、build-essentialというパッケージにgccなどのコンパイラを含むプログラムを実行するために必要なツールを入れます。
ターミナルで以下のコマンドを入れてみましょう。  

```
$ sudo apt-get install build-essential
```

これでC/C++のインストールが完了するはずです！  
僕の環境ではこのコマンドを実行したときエラーが出ました。
これについては、別記事にて書いていきます。  

# 2. HelloWorldする

それでは無事C/C++の環境構築ができたところで、HelloWorldをしましょう！  

## C

まずはC言語のHelloWorldをやっていきましょう。  
以下のコードを入力してください。  

```C:hello.c
#include <stdio.h>

int main(void)
{
    printf("Hello World!\n");

    return 0;
}
```

そして、ターミナルでファイルを保存したディレクトリを開き、以下のコマンドを実行するとHello World完了です！  

```
$ gcc -o hello hello.c
$ ./hello
Hello World!
```

また、1行でコマンドを書くこともできます。  

```
$ gcc -o hello hello.c && ./hello
Hello World!
```

![2-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/2005d399-0931-0934-64d3-b745d5556d80.png)

## C++

次はC言語のHelloWorldをやっていきましょう。  
以下のコードを入力してください。  

```C++:hello.cpp
#include <iostream>

int main()
{
    std::cout << "Hello World!\n" << std::endl;

    return 0;
}
```

そして、ターミナルでファイルを保存したディレクトリを開き、以下のコマンドを実行するとHello World完了です！  

```
$ g++ -o hello hello.cpp
$ ./hello
Hello World!
```

また、Cと同様に1行でコマンドを書くこともできます。  

```
$ g++ -o hello hello.cpp && ./hello
Hello World!
```

![2-2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/88dc6c0d-eee3-fb42-20ec-84cb93d74eaa.png)

# まとめ

この記事では、C/C++言語の開発環境を構築し、Hello Worldをしました！  
僕の環境は特殊だったためめんどくさいエラーが出てしまいました。  
これについては別記事にまとめていくので、よかったらまた見てください〜。  

# 参考記事

* [How to Run C/C++ Programs in Linux [Terminal & Eclipse]](https://itsfoss.com/c-plus-plus-ubuntu/)