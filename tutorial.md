# keypadを利用した数あてゲームを作ろう（１けたの数あて）
```package
keypad=github:lioujj/pxt-keypad
```

## keypadを利用した数あてゲームを作ろう@showdialog
この活動では、keypad を入力デバイスに使った、数あてゲームを作ります。

![メインイメージ](https://github.com/SKYTREE-1/guess-the-number/blob/master/guess-the-number.png?raw=true)

---

## 1. 接続しよう@showdialog

まず、テープLED と keypad を micro:bit に接続しましょう。

**配線2**
- micro:bit → keypad
  - P0 → R1
  - P1 → R2
  - P2 → R3
  - P8 → R4
  - P13 → C1
  - P14 → C2
  - P15 → C3
  - P16 → C4

👉 kiypad を上にして、左から番号を振っています。

![配線図2](https://raw.githubusercontent.com/SKYTREE-1/keypad1/3ee6caef52b3e570270b9936053c9ac69e8b202b/images/wiring-diagram_keypad.png?raw=true)
 

## 1. 接続しよう（キーパッドの初期化） @showdialog
キーパッドを接続したら、``||keypad:KeyPad||``にある``||keypad:set 4*4 KeyPad〜||``を``||basic:最初だけ||`` にセットして、次のようにセットします。

  - pin1（R1）→ P0
  - pin2（R2）→ P1
  - pin3（R3）→ P2
  - pin4（R4）→ P8
  - pin5（C1）→ P13
  - pin6（C2）→ P14
  - pin7（C3）→ P15
  - pin8（C4）→ P16

```blocks

keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)

```

## 2.変数の作成 @showdialog
数あてのプログラムでは、次のような変数を利用します。

1. secret  : 秘密の数（←これが正解になります）
2. guess　: ゲームをしている人が考えた数
3. chara : キーパッドからの入力された数
4. status : 状態を表す変数

1〜3 は文字列、4は数の変数として扱います。（4については、後で詳しく説明します。）

## 2. 変数の作成
``||variables:変数||`` の``||variables:変数を追加する...||``で、新しい変数  **secret**,**guess**, **chara**, **status** を作成します。
４つの変数を作成したら、次へ進んでください。

## 3.状態（モード）の設定（導入） @showdialog

これから作るプログラムでは、プログラムでは 次の３つの状態（モード） を考えます。

 - 待機状態（Standby）
    - 何も入力を受け付けない状態です。ボタンが押されるまで待っています。
 - 入力受付状態（Input）
    - ボタンやセンサーからの入力を受け付ける状態です。入力があると処理を実行したり、データを記録したりできます。
 - 判定まち状態（Waiting）
    - ボタン入力が終わり、正解かどうかの判定を待っています。

ポイント：
プログラムはこの状態を 変数 status を利用して表し、status が変わることで、「今何をしているか」を判断します。

## 3.状態（status）の設定（導入２） @showdialog
状態は 変数 status で管理します。

- status = 0  … 待機状態(standby)
- status = 1 … 入力受付状態（input）
- status = 2 … 判定まち状態（Waiting）
この変数によって、プログラムが動作を変えます。

status 0 から 1 のの切り替えには、Aボタンを割り当て、待機モード → 入力受付モード というように切り替えるようにします。

では、プログラムを作りましょう。

## 2. 状態（status）の初期設定
最初だけに ``||variables:変数 mode を 0 にする||`` をセットします。

```blocks
keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)
let status = 0
```

## 2. 状態（status）の設定（statusの切り替え1）
``||input:ボタンAが押されたとき||`` をだします。
``||logic:論理||`` にある、フォークの形のブロックを``||input:ボタンAが押されたとき||`` にセットして、条件のところが **status = 0** となるようにブロックをセットします。

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
    	
    } else {
    	
    }
})
```
## 2. 状態（status）の設定（statusの切り替え2）
``||input:ボタンAが押されたとき||`` のなかの条件分岐の **status = 0**  後に ``||variables:変数||``から、``||variables:変数 status を 0 にする||`` をセットして、0を１に変えます。


```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1

    } else {

    }
})
```

## 2. 状態（status）の設定（statusの切り替え）
``||input:ボタンAが押されたとき||``のなかの条件分岐の **でなければ**の後に、``||variables:変数 status を 0 にする||``をセットします。



```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
    } else {
        mode = 0
    }
})

```

## 2. 状態（status）の確認
状態をチェックするために、Bボタンを押したら、status が確認できるようにします。
``||input:ボタンAが押されたとき||`` を、もうひとつ出し、**A** を **B** に変更します。
``||input:ボタンBが押されたとき||`` の中に、``||basic:基本||`` にある ``||basic: 数を表示||`` をいれ、数の部分に ``||variables:変数||`` にある ``||variables:status||`` をセットします。

```blocks
input.onButtonPressed(Button.B, function () {
    basic.showNumber(status)
})
```

## 2. 状態（モード）の設定（テスト） @showdialog
ここまでのプログラムです。


```blocks

input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
    } else {
        mode = 0
    }
})
let status = 0
keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)

input.onButtonPressed(Button.B, function () {
    basic.showNumber(status)
})

basic.forever(function () {
	
})

```
##  2.状態（モード）の設定（テスト） 
micro:bit にダウンロードして実際に動かしてみましょう。
A ボタンを押したあと、Bボタンを押して、状態を表す status の表示が切り替わることが確認できたら、次へ進みましょう。

## 3.キー入力を受け取る @showdialog
待機状態(0)と入力受付状態(1)の切り替えができたら、入力受付状態の時に``||basic:ずっと||`` キー入力を受け付けるようにします。
つまり、statusが「入力受付状態（``||variables:status=1||``）」の時だけ、キー操作に対して、入力処理が実行されるようにします。

[入力処理の流れ]

1. キーパッドから受け取った数は、まず、変数**chara** に入れます。
2. 確認のため画面に **chara** を表示させます。
3. **chara** に数字が入ったことを確認して、**guess** に **chara** をいれます。
4. 3 のあと、**status** を **2** に変えます。

最後に、 ``||variables:status = 2||`` に変更することで、入力受付状態から、判定待ち状態になります。



## 3.キー入力を受け取る（入力の受付1）
``||basic :ずっと||`` ブロックに、``||logic:論理||``の``||logic:もし〜なら〜でなければ||`` ブロックをセットして、条件が ``||logic: status  = 1||``になるようにします。  

```blocks
basic.forever(function () {
    if (status == 1) {

        
    } else {
    	
    }
})
```

## 3.キー入力を受け取る（入力の受付2）
``||variables:変数を（　）にする||`` をセットして``||variables:変数||`` の部分を ``||variables:chara||`` にかえ、空欄に、``||keypad:KeyPad value||`` ブロックをセットします。



```blocks
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
         
    } else {
    	
    }
})
```

## 3.キー入力を受け取る（入力の受付3）
``||variables:charaを KeyPad value にする||``の下に``||basic:基本||``の``||basic:文字列を表示（　）||``をセットして、空欄部分に ``||variables:変数||``にある``||variables:chara||``をいれます。
また、その下に ``||basic:基本||`` の``||basic:一時停止||`` を入れ、時間を **300ミリ秒** にします。


```blocks
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
     } else {
    	
    }
})
```


## 3.キー入力を受け取る（入力の受付4）
``||logic:論理||`` から ``||logic:もし〜なら||`` ブロックを追加して、条件の部分を  ``||logic: chara ≠ （""） ||`` となるように設定します。
**""** は、``||advanced:高度なブロック||`` にある ``||text:文字列||`` の中にあります。


```blocks
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
        if (chara != "") {

        }
    } else {
    	
    }
})

```

## 3.キー入力を受け取る（入力の受付5）
新しい条件の下に ``||variables: 変数を 0 にする||`` をいれて、変数を **guess** に変更して 0 の部分に、``||variables:変数||``にある **chara** をセットします。
その後に、 ``||variables: 変数を 0 にする||`` をいれてて、変数を変数を **status** に変更して 0 の部分に、**2** をかきいれます。


```blocks
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
        if (chara != "") {
            guess = chara
            status = 2
        }
    } else {
    	
    }
})

```

## 3.キー入力を受け取る（テスト） @showdialog
ここまでのプログラムです。
キー入力の後に一時停止を入れるのは、連続して同じ入力が何度も処理されてしまうのを防ぐためです。

micro:bitなどではループがとても速く回るため、ボタンが押されたままの一瞬の間にも、同じ入力が何十回も検出されてしまいます。
少し待ち時間を入れることで、**「1回の押下＝1回の入力」**として安定した動作にできます。

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
         status = 1
    } else {
        status = 0
    }
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(status)
})
let chara = ""
let guess = ""
let secret = ""
let status = 0
basic.showIcon(IconNames.Heart)
keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
        if (chara != "") {
            guess = chara
            status = 2
        }
    } else {
    	
    }
})

```

## 3.キー入力を受け取る（テスト） 
ここまでできたら、micro:bit にダウンロードして実際に動かしてみましょう。
Bボタン → Aボタン → Bボタン → keypad のキー（どれか１つ）→ Bボタン

の順に押して、status 表示が切り替わることや、keypad から入力した数字が表示されることが確認できたら、次へすすんでください。
（入力待ち状態の時にBボタンを押したとき status 表示が一瞬です。）

## 4.答え合わせをする（説明）  @showdialog

状態の切り替えができるようになったら、Aボタンを押されたときに、入力された数字が 問題 **secret** と一致するか確認するプログラムを作成します。

Aボタンが押された時に、status = 0 の場合の処理はすでにかきました。
ここでは、新しく条件を追加して、status = 2 の場合について次のようにプログラムを作成します。

- Aボタンが押された時に、status = 2 なら次のことをする
    - guess と secret が一致したら
       - スマイルマークを表示
       - status を 0にする
    - それ以外なら 
       - X を表示
       - guessの内容をクリア
       - status を1にする

## 4.答え合わせをする（出題する1）
判定する前に、問題をつくりましょう。
``||input:ボタンAが押されたとき||`` の中の``||logic:もし〜なら〜でなければ||``ブロックの ``||variables:status を1にする||`` の上に
``||variables:変数を（）にする||``をおき、``||variables:変数||`` を **secret** にします。
```blocks 
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = 0
        status = 1
       
    } else {
        status = 0
    }
})

```


## 4.答え合わせをする（出題する2）
判定する前に、問題をつくりましょう。
``||variables:secretを（）にする||``に、``||advanced:高度なブロック||`` の ``||text:文字列||`` にある``||text:数値（）を文字列にする|`` をいれて、（）に ``||math:計算||``の``||math:()から()の乱数||``をセットして、**0から9の乱数** になるようにします。 

```blocks 
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
       
    } else {
        status = 0
    }
})

```

## 4.答え合わせをする（条件の追加）
``||input:ボタンAが押されたとき||`` の中の``||logic:もし〜なら〜でなければ||``ブロックの左下の **+** をクリックします。
新しく追加された条件の部分を ``||logic: status = 2 ||``となるようにします。

```blocks 
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
    
    } else {
        status = 0
    }
})

```



## 4.答え合わせをする（判定１）
新しい条件のすぐ下に ``||logic:もし〜なら〜でなければ||``ブロックを追加して、条件の部分が ``||logic: guess = secret ||`` となるようにします。

```blocks 
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {

        } else {

        }
    } else {
        basic.showString("error")
        status = 0
    }
})

```


## 4.答え合わせをする（判定２）
新しく追加した条件の下に、``||basic:アイコンを表示||``を使ってスマイルマークを表示するようにして、
その次に ``||variables: status を 0||`` にするをセットします。


```blocks 
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0

        } else {

        }
    } else {
        basic.showString("error")
        status = 0
    }
})


```


## 4.答え合わせをする（判定３）
でなければ下に、``||basic:アイコンを表示||``を使って x を表示するようにして、
その次に``||variables: guess を （""）||`` と ``||variables: status を 1||`` にするをセットします。
**（""）** は、``||advanced:高度なブロック|`` の``||text:文字列||`` の中にあります。
```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0
        } else {
            basic.showIcon(IconNames.No)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
```


## 4. 判定 テスト @showdialog
ここまでのプログラムです。

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0
        } else {
            basic.showIcon(IconNames.No)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(status)
})
let chara = ""
let guess = ""
let secret = ""
let status = 0
basic.showIcon(IconNames.Heart)
keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
        if (chara != "") {
            guess = chara
            status = 2
        }
    } else {
    	
    }
})
```
## 4. 判定 テスト
ここまでできたら、micro:bit にダウンロードして実際に動かしてみましょう。
キーパッドは、平らなところにおいて、ゆっくり押さえるようにしてください。

## 🌈 ここまでのプログラムを振り返ろう！@showdialog
ここまでで、この数あてゲームは、プレイヤーが入力した数字に対して「あたり」または「はずれ」と結果を表示するところまで完成しています。
そして、プレイヤーは当たるまで、数字を入力していくようになっています。

しかし、このままでは正解にたどり着くまでに手がかりがなく、遊びにくい状態です。
そこで、プレイヤーがより楽しみながら正解を探せるように、「ヒントを出す機能」を追加ししましょう。
具体的には、入力した数が正解より大きいか小さいかを教えるようにして、次の予想に役立てられるようにします。

あたりを判定する部分で、いまは、あたり・はずれの２通りに分けていますが、つぎのように３つの場合にわけてヒントをだします。

 - guess = secret なら スマイルマークを表示
 - guess < secret なら Xを表示した後、↑ を表示
 - それ以外（guess > secret）なら Xを表示したあと、↓を表示

あたりの場合、最後に status = 0 にすること、はずれの場合、guess をリセットして status = 1 に変更する部分はかわりません。

では、プログラムの続きを作成して数あてを完成させましょう。



## 5.ヒントを出す（場合を増やす1）
``||input:ボタンAが押されたとき||`` の中の **status = 2 ** の場合の処理について考えます。
``||logic:もし guess=secret なら||``ではじまるブロックの左下の ** + ** をクリックして条件を増やします。 

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0
        } else if(false) {
        } else {
            basic.showIcon(IconNames.No)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
```
## 5.ヒントを出す（場合を増やす2）
新しく増えた条件の部分が ``||logic:guess < secret ||`` となるようにして、
処理の部分は ``||logic:でなければ ||``の中と同じになるようにブロックを配置します。 

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0
        } else if(guess < secret) {
            basic.showIcon(IconNames.No)
            guess = ""
            status = 1
        } else {
            basic.showIcon(IconNames.No)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
```

## 5.ヒントを出す（ヒントを出す）
``||logic:guess < secret ||`` の ``||basic: アイコンを表示 X ||`` ブロックの下に、``||basic:基本||``にある ``||basic:矢印を表示||``をセットして、矢印の向きを **上向き ↑** にします。
また、同じように、``||logic:でなければ ||`` （guess > secret となるとき）に、下向きの矢印を表示するようにします。

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            
            status = 0
        } else if(guess < secret) {
            basic.showIcon(IconNames.No)
            basic.showArrow(ArrowNames.North)
            guess = ""
            status = 1
        } else {
            basic.showIcon(IconNames.No)
            basic.showArrow(ArrowNames.South)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
```

## 5. ヒントの追加 テスト @showdialog
ここまでのプログラムです。

```blocks
input.onButtonPressed(Button.A, function () {
    if (status == 0) {
        secret = convertToText(randint(0, 9))
        status = 1
    } else if (status == 2) {
        if (guess == secret) {
            basic.showIcon(IconNames.Happy)
            status = 0
        } else if(guess < secret) {
            basic.showIcon(IconNames.No)
            basic.showArrow(ArrowNames.North)
            guess = ""
            status = 1
        } else {
            basic.showIcon(IconNames.No)
            basic.showArrow(ArrowNames.South)
            guess = ""
            status = 1
        }
    } else {
        status = 0
    }
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(status)
})
let chara = ""
let guess = ""
let secret = ""
let status = 0
basic.showIcon(IconNames.Heart)
keypad.setKeyPad4(
DigitalPin.P0,
DigitalPin.P1,
DigitalPin.P2,
DigitalPin.P8,
DigitalPin.P13,
DigitalPin.P14,
DigitalPin.P15,
DigitalPin.P16
)
basic.forever(function () {
    if (status == 1) {
        chara = keypad.getKeyString()
        basic.showString(chara)
        basic.pause(300)
        if (chara != "") {
            guess = chara
            status = 2
        }
    } else {
    	
    }
})
```

## 🌈 全体をプログラムを振り返ろう@showdialog

お疲れ様でした！　
数あてゲームの完成です。ぜひ遊んでみてください。

また、以下のような工夫をするとより楽しくなると思います。

- **あたり！** を派手に演出する
- **入力を求める画面（プロンプト）** の工夫
- **得点** をつける
- **◯回までチャレンジできる**というような制限をつける

他にも、ゲームを楽しむ工夫をいろいろ考えてプログラムにチャレンジしてみてください！

最後に、全体の流れを、まとめたので確認してみてください。

![全体の流れ](https://github.com/SKYTREE-1/guess-the-number/blob/master/guess-the-number1.jpg?raw=true)



```