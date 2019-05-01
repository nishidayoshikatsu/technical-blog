# はじめに

この記事では、  
1. UbuntuにGo言語の開発環境をインストールする
2. HelloWorldする  
の2つについて書いていきまーす

# 前提環境

* Ubuntu 18.04.2 LTS

# 1. UbuntuにGo言語の開発環境をインストールする

ctrl+alt+t キーを押すことでターミナルを開きます。  
Ubuntuでターミナルを開いて以下のコマンドを入力します。  
```
$ sudo apt install golang
```
これでgoのインストールが完了するはずです！  

![1-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/e242b6c3-bed5-a226-6f77-8f8c6bb24ba4.png)

次は、ちゃんとgoの開発環境が入ってるか確認します。  
以下のコマンドを実行することで、現在入っているgoのバージョンを確認できます。  

```
$ go version
go version go1.10.4 linux/amd64
```

![1-2.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/19bbfa61-5ebf-70d9-d107-b5049121a538.png)

# 2. HelloWorldする

それでは無事goの環境構築ができたところで、HelloWorldをしましょう！  
以下のコードを入力してください。  

```go:hello.go  
package main

import "fmt"

func main() {
  fmt.Printf("Hello World!\n")
}
```

そして、ターミナルでファイルを保存したディレクトリを開き、以下のコマンドを実行するとHello World完了です！  

```
$ go run hello.go
Hello World!
```

![2-1.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/9aeee302-32f2-db9e-a2ba-58c9780cb7a0.png)

# まとめ

この記事では、Go言語の開発環境を構築し、Hello Worldをしました！  
今後もGoに関連する記事を書いていくのでよろしくおねがいします！

# 参考記事

* [go run と go buildの違い](http://nununu.hatenablog.jp/entry/2016/09/20/210000)
* [初めてのgo！インストールまで。](https://qiita.com/inexp_eng4432/items/08dce692894c92ae08ee)