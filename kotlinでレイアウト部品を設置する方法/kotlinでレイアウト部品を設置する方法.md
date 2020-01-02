# はじめに

こんにちは！[@nsd244](https://twitter.com/nsd244)です。
この記事では、Androidアプリ開発でkotlinを使った場合に動的にレイアウト部品を設置する方法について説明していきます。

# 前提環境

* Android-studio: 3.5.3
* kotlin: 1.3.61
* SDK version: 29
* Gradle version: 5.4.1

# kotlinでレイアウトに部品を設置する方法

サンプルコードを[github](https://github.com/nishidayoshikatsu/KotlinLayout)にあげましたので、参考にしてください。  


以下のコードのonCreateメソッドの中に追記していく形で各要素を設置していきます。
基本的には、レイアウトをxmlで定義すれば、他のButtonなどの要素はレイアウトの中に追加して書いていくことができます。
部品をkotlinで追加することができれば、動的なレイアウトを作成することが可能になります。

```kotlin:MainActivity.kt

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)  // レイアウトファイルの読み込み
    }
}

```

```XML:activity_main.xml

<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/MainLinearLayout"
        android:orientation="vertical"
        xmlns:tools="http://schemas.android.com/tools"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        tools:context=".MainActivity">
</LinearLayout>
```

## TextView

```kotlin:MainActivity.kt

val linearLayout = findViewById(R.id.MainLinearLayout) as LinearLayout    // レイアウトファイルにあるレイアウトのidを指定して読み込みます
linearLayout.gravity = Gravity.CENTER   // 画面中央寄せ

val text = TextView(this)
text.text = "ここにテキスト"
text.gravity = Gravity.CENTER
text.textSize = 35F

linearLayout.addView(text)  // レイアウトファイルにテキストビューを追加します

```

## RadioGroup・RadioButton

RadioButtonは、RadioGroupの中に要素を追加していきます。  
ここでは、配列にテキストを格納しておいて、配列の長さ分のRadioButtonを生成しています。  



```kotlin:MainActivity.kt

val linearLayout = findViewById(R.id.MainLinearLayout) as LinearLayout    // レイアウトファイルにあるレイアウトのidを指定して読み込みます
linearLayout.gravity = Gravity.CENTER   // 画面中央寄せ

val radioGroup = RadioGroup(this)
radioGroup.gravity = Gravity.CENTER
radioGroup.orientation = RadioGroup.VERTICAL

val list: List<String> = listOf("サンプル1", "サンプル2", "サンプル3", "サンプル4", "サンプル5")

list.forEach{
    val radioButton = RadioButton(this)
    radioButton.text = it
    radioButton.setPadding(0, 30, 0, 30)
    radioGroup.addView(radioButton) // radioGroupにradioButtonを追加する
    if(it.equals(list[0])){
        radioButton.isChecked = true
    }
}
```

また、選択されているRadioButtonのtextの中身を見るためには、以下のコードで実現できます。  

```kotlin:MainActivity.kt
val id: Int = radioGroup.checkedRadioButtonId
val radiotext: String = radioGroup.findViewById<RadioButton>(id).text.toString()
```

## Button

```kotlin:MainActivity.kt

val linearLayout = findViewById(R.id.MainLinearLayout) as LinearLayout    // レイアウトファイルにあるレイアウトのidを指定して読み込みます
linearLayout.gravity = Gravity.CENTER   // 画面中央寄せ

val button = Button(this)
button.text = "ここにテキスト"

linearLayout.addView(button)
```

Buttonをクリックした際の処理は、

```kotlin:MainActivity.kt

button.setOnClickListener {
	// ここにクリック時の処理を記述する
}

```


# まとめ

kotlinはあまりネットなどに資料が落ちてなくて苦労したので、少しずつ僕の知見をいろいろブログで書いていければなと思っています。  
kotlinで書いていくには、[公式ドキュメント](https://kotlinlang.org/docs/reference/)が一番参考になります。  