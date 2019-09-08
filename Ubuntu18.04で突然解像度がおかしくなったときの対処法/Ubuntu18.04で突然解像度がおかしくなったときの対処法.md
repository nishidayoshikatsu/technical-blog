# はじめに
僕のPCはubuntu18.04とWindows10でデュアルブートしてありり、もともとのPCの解像度は1920×1080です。  

```
sudo apt-get update
sudo apt-get upgrade
```

↑のコマンドのようにパッケージの更新をかけ、再起動したら解像度が640×480で以下のように固定になっていました。  
他のカーネルをすべて試しましたが、同様の現象が起きていました。  
その問題に対する対象法について説明していきます。  

![DSC_0800.JPG](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/245c0754-5523-ebbb-7901-582c9fb2a76c.jpeg)

ご意見・ご要望などある方はぜひコメントよろしくお願いします。

# 現状の確認

まずは、現状の解像度の状態を確認しましょう。以下のコマンドを実行します。  

```
sudo apt-get install inxi
inxi -G
```

すると、以下の画像のResolutionの部分に解像度が書いてあると思います。  


![範囲を選択_029.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/c80de830-585e-70e4-9872-77dcd70714da.png)  

また、解像度をあやつるコマンドである"xrandr"も用いてみます。  

```
xrandr
```

以下の画像のように、Screen 0が800×600の解像度としか認識されていないことがわかります。

![範囲を選択_030.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/d75dafa5-0789-07b2-d958-3b208983ae87.png)  

この現象に対する対処法として、以下のようなものが挙げられます。  
（他にあれば教えてください）  

* xrandrに手動で新しい解像度を追加する
* grubのファイルを直接いじって解像度を変更する

結論から言うと、xrandrで解像度を追加する方はうまく行かず、grubのファイルを直接いじることで解決しました。  

# xrandrに手動で新しい解像度を追加する（失敗）

まずは、xrandrに手動で新しい解像度を追加してみます。  
始めに行っておくと、これは失敗しました。  
てっとりばやく問題を解決したい人は次の項を見てみたほうがいいかもしれません。  

実行コマンドは、以下のようになります。  
（戻したい解像度に応じて数字は変更してください）  
最初のコマンドで出てきた文字列の一部を次のコマンドの"1920×1080_60.00"の部分に適宜変更してください。  
（Screen 0の部分についてもxrandrで出てきたスクリーン名に変更する。）  

```
cvt 1920 1080
sudo xrandr --addmode Screen 0 "1920x1080_60.00"
```

サイズが取得できないらしい。  
原因を調べると、grubをいじれば解決するらしい（次の項参照）  


![範囲を選択_032.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/8216b429-1c9c-3828-8516-8595036166a7.png)

# grubのファイルを直接いじって解像度を変更する

最終手段（？）として、grubのファイルをいじって解像度の変更を試みました。  
vimコマンドを実行し、grubのファイルを編集します。


```
sudo vim /etc/default/grub
```

![範囲を選択_034.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/f41092c9-23c2-c474-2be8-ca4b0357a19d.png)

そして、ファイル内に以下の文を追加します。  
（追加の仕方は、vimコマンドでググればでてきます。）  

```
GRUB_GFXMODE=1920x1080
```

![範囲を選択_033.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/3ba38a90-e9c0-07af-1fc6-5988f8bc4a96.png)

最後に、以下のコマンドでgrubファイルを更新し、再起動をかけることで解像度がもとの解像度に戻るはずです！！！

```
sudo update-grub
```

![範囲を選択_035.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/289151/f16d8699-cf6f-f064-7f04-8f4ce3fcec56.png)

# まとめ
この記事では、突然Ubuntuの解像度がおかしくなったときの対処法について書いてきました！
他にも疑問があれば、是非コメント欄や[twitter](https://twitter.com/nsd244)などで話しかけてくれると嬉しいです！  

# 参考サイト
* [Failed to get size of gamma for output default when trying to add new screen resolution](https://askubuntu.com/questions/441040/failed-to-get-size-of-gamma-for-output-default-when-trying-to-add-new-screen-res)  
* [Ubuntu日本語フォーラム / モニタの解像度を変更する](https://forums.ubuntulinux.jp/viewtopic.php?id=11178)  
* [ディスプレイの設定をコマンドライン・シェルで行う](https://qiita.com/hiro_mayo/items/db8ddcea600e8bfbdd5f)  
* [Ubuntu 18.04 で解像度が見つからなくなったときの対処](https://blog.capilano-fw.com/?p=1881)