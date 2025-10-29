# keypad でLEDの色をコントロールしよう（発展編）
```package
keypad=github:lioujj/pxt-keypad
```

## keypadを利用した数あてゲームを作ろう@showdialog
この活動では、keypad を入力デバイスに使った、数あてゲームを作ります。

![メインイメージ](https://www.kodai.uec.ac.jp/sk/make-code/np/img_neopixel.png)

---

## 1. 接続しよう1@showdialog

まず、テープLED と keypad を micro:bit に接続しましょう。

**配線1**
- micro:bit → テープLED（NeoPixel）  
  - 3V → +5V
  - P12 → DIN
  - GND → GND

👉 接続部分を上にして左から+5V, DIN, GND。（黄色の線は信号、赤/白はモジュールごとに違うので注意‼）。  

![配線図１](https://www.kodai.uec.ac.jp/sk/make-code/np/img_wiring_uecsc.png)
  写真では、真ん中の線（DIN）をP0に接続するように書いてありますが、P12に接続してください。

## 1. 接続しよう2 @showdialog

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
 

## 1. 接続しよう2（キーパッドの初期化） @showdialog
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

## テープLEDを光らせよう1　LEDの個数の設定
1. テープLEDの点灯テスト

はじめに、テープLEDを光らせるテストをします。
``||neopixel: NeoPixel ||`` にある ``||variables:変数 strip を〜||``を``||basic:最初だけ||``にいれて``||neopixel: 端子P0に接続しているLED24個の〜 ||``の端子を P12に変更して、「24」を「8」にかえます。  

```blocks
let strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
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

## テープLEDを光らせよう2　全部赤で光らせる
``||input:入力||``から``||input:ボタンAが押されたとき||``をだして、``||neopixel:strip を赤色に点灯する||``をいれる。

   ```blocks
   input.onButtonPressed(Button.A, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.Red))
})

  let strip: neopixel.Strip = null
   ```


## テープLEDを光らせよう3　テスト@showdialog
ここまでのプログラムがです。
   ```blocks
let strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
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

   input.onButtonPressed(Button.A, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.Red))
    })
    

  ```
## テープLEDを光らせよう3　テスト
ここまでできたら、micro:bit にダウンロードして動かしてみよう。


## 2.状態（モード）の設定（導入） @showdialog

これから作るプログラムでは、プログラムでは 次の2つの状態（モード） を考えます。

 - 待機モード（Standby）
    - 何も入力を受け付けない状態です。ボタンが押されるまで待っています。
 - 入力受付モード（Input）
    - ボタンやセンサーからの入力を受け付ける状態です。入力があると処理を実行したり、データを記録したりできます。


ポイント：
プログラムはこの モードの切り替え によって「今何をしているか」を判断します。

## 2.状態（モード）の設定（導入２） @showdialog
モードの状態は 変数 mode で管理します。

- mode = 0  … 待機モード(standby)
- mode = 1 … 入力受付モード（input）

この変数によって、プログラムが動作を変えます。

モードの切り替えには、Aボタンを割り当て、待機モード ⇄ 入力受付モード というように切り替えるようにします。

では、プログラムを作りましょう。

## 2. 状態（モード）の設定（変数の作成）
``||variables:変数||`` の``||variables:変数を追加する...||``で、新しい変数 **mode** を作成します。
また、最初だけに ``||variables:変数 mode を 0 にする||`` をセットします。

```blocks
let mode = 0
```

## 2. 状態（モード）の設定（modeの切り替え）
``||input:ボタンAが押されたとき||`` をだします。
``||logic:論理||`` にある、フォークの形のブロックを``||input:ボタンAが押されたとき||`` にセットして、条件のところが **mode = 0** となるようにブロックをセットします。

```blocks
input.onButtonPressed(Button.A, function () {
    strip.showColor(neopixel.colors(NeoPixelColors.Red))
    if (mode == 0) {
    	
    } else {
    	
    }
})
let strip: neopixel.Strip = null
```
## 2. 状態（モード）の設定（modeの切り替え）
``||input:ボタンAが押されたとき||`` のなかの条件分岐の **mode = 0**  後に ``||variables:変数||``から、``||variables:変数 mode を 0 にする||`` をセットして、0を１に変えます。
その後に、``||basic:基本||`` にある``||basic:数を表示（　）||`` を入れ、0の部分に``||variables:mode||`` をあてはめます。
また、``||neopixel:strip を赤色に点灯する||``を、``||basic:数を表示（　）||``の下に移動します。

```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Red))
    } else {

    }
})
let strip: neopixel.Strip = null
```

## 2. 状態（モード）の設定（modeの切り替え）
``||input:ボタンAが押されたとき||``のなかの条件分岐の **でなければ**の後に、``||variables:変数 mode を 0 にする||``をセットします。
その後に、``||basic:基本||`` にある``||basic:数を表示（　）||`` 追加して、0の部分に``||variables:mode||`` をあてはめます。
また、``||neopixel:strip をblackに点灯する||``を、``||basic:数を表示（　）||``の下に追加します。

```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Red))
    } else {
        mode = 0
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
    }
})
let strip: neopixel.Strip = null
```

## 2. 状態（モード）の設定（テスト） @showdialog
ここまでのプログラムです。


```blocks

input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Red))
    } else {
        mode = 0
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
    }
})
let mode = 0
let strip: neopixel.Strip = null
strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
mode = 0
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
	
})

```
##  2.状態（モード）の設定（テスト） 
micro:bit にダウンロードして実際に動かしてみましょう。
A ボタンを押すことで、モードの表示が切り替わることが確認できたら、次へ進みましょう。

## 3.キー入力を受け取る @showdialog
待機モード(0)と入力受付モード(1)の切り替えができたら、入力受付モードの時に、
ードが「入力受付モード」になったら、``||basic:ずっと||`` の中でキー入力を受け付けるようにします。
つまり、モードが「入力受付モード（``||variables:mode=1||``）」の時だけ、キー操作による入力処理が実行されるようにします。

キーパッドから受け取った数は、まず、変数**c** に入れてから、チェックして 変数 **color** の末尾に付け加えるようプログラムをします。
ここで受け取った数は、文字列型で渡されるので、必要に応じて、数値に変換して利用します。


## 3.キー入力を受け取る（変数の作成）
``||variables:変数||`` の``||variables:変数を追加する...||``で、新しい変数**c** と **color** を作成します。
そして、最初だけに ``||variables:変数 〜 を 0 にする||`` を２個セットして、変数名を **c** と **color** にかえ、 ``||advanced:高度なブロック||``にある``||text:文字列||``の一番上にある``||text:" "||``をセットします。
（1は文字列として扱われます。）

```blocks
let mode = 0
let strip: neopixel.Strip = null
strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
mode = 0
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
let c = ""
let color = ""
```

## 3.キー入力を受け取る（入力の受付1）
``||basic:ずっと||`` に、``||logic:論理||``にあるフォークの形の``||logic:もし〜なら||``ブロックをセットして、
条件の部分を``||variables:mode||`` =  1　とします。

```blocks
basic.forever(function () {
    if (mode == 1) {
    	
    } else {
    	
    }
})
```

## 3.キー入力を受け取る（入力の受付2）
``||logic:もし〜なら||``ブロックの ``||variables:mode||`` =  1　の下に、``||variables:cを（　）にする||``をセットします。 

```blocks
basic.forever(function () {
    if (mode == 1) {
        c = 0
    } else {
    	
    }
})

```

## 3.キー入力を受け取る（入力の受付3）
``||variables:cを 0 にする||`` の 0 の部分に、``||keypad:KeyPad value(string)||``をいれます。
代入のブロックの後に、``||basic:一時停止（ミリ秒）||``を入れます。時間は300ミリ秒にしてください。


```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
    } else {
    	
    }
})

```

## 3.キー入力を受け取る（入力の受付4）
``||basic:一時停止（ミリ秒）||``の後に、``||basic:文字列を表示（　）||`` に ``||variables:c||`` をセットして代入した文字列を表示します。


```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
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
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Red))
    } else {
        mode = 0
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
    }
})
let mode = 0
let strip: neopixel.Strip = null
strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
mode = 0
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
let c = ""
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
    } else {
    	
    }
})
```

## 3.キー入力を受け取る（テスト） 
ここまでできたら、micro:bit にダウンロードして実際に動かしてみましょう。
キーパッドは、平らなところにおいて、ゆっくり押さえるようにしてください。

## 4.カラーコード作る（説明）  @showdialog

- LEDを光らせるカラーコードを rgb という配列に収めます。（初期状態は空の配列にします。）
- キーパッドの入力から数を作り、赤・緑・青の順に決めます。
- それぞれの色の色番号の区切れ目には「A」のキーを押します。
- Aが押されたら、前から順に数を書き換えます。

## 4.カラーコード作る（配列の作成）
最初に カラーコードを入力する配列を作ります。
``||advanced:高度なブロック||``にある ``||arrays:配列||``の``||arrays:変数〜を〜にする||`` を``||basic:最初だけ||`` に追加して、** - ** を２回押して **空の配列**
にします。

```blocks 
let 配列 = ""

```


## 4.カラーコード作る（配列の作成2）
配列名をクリックして``||variables:変数の名前を変更||`` をクリックして、変数名を **rgb** に変えます。

```blocks 
let rgb = ""

```

## 4.カラーコード作る（追加説明）@showdialog
次に、 c = A で色を確定、それ以外で、色の入力を継続するプログラムを作ります。
具体的には、以下のようにします。

 -  **c = "A" ** の時に、rgb の**カウンター**番目に color を数に変換したものにする
 -  **それ以外** の時に、c を color の末尾に追加する

 ここでは、問題を簡単にするために間違えて、他の文字が入ったりすることは想定していません。
 やり直しもできないので、やり直すときは、リセットして入力をはじめます。

## 4.カラーコード作る（色の入力3）
**mode=1** の下に、新しく``||logic:もし〜なら||`` ブロックを追加して条件を ** c = "A"** とします。
条件を作るときは、先に右辺に``||text:文字列||``にある``||text:（" "）||``ブロックをセットして文字を**A**を入力してから、左辺に``variables:c||``を入れます。
（順番が違うと、途中でエラーメッセージが表示されます。）

```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {

        } else {
        }
    } else {
    	
    }
})
```


## 4.カラーコード作る（色の入力3）
新しく追加した``||logic:もし〜なら||`` ブロックの ** c = "A"** の次に、``||arrays:配列||``から、``||arrays:配列の最後に（）を追加する||``を選びセットして、変数名``||arrays:配列||``を
``||arrays:rgb||``に変更します。
```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push()
        } else {

        }
    } else {
    	
    }
})
```

## 4.カラーコード作る（色の入力4）
新しく追加した ``||arrays:rgbの最後に（）を追加する||``空欄に||text:テキスト||``の``||text:文字列（123）を数値に変換する||``を入れ、そこに``||variables:color||``を入れます。


```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push(parseFloat(color))
            
        } else {
        
        }
    } else {
    	
    }
})
```

## 4.カラーコード作る（色の入力5）
そして、``||variables:color||`` を ``||text:（" "）||`` ブロックを使って初期状態に戻します。

```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push(parseFloat(color))
            color = ""
        } else {
            
        }
    } else {
    	
    }
})
```
## 4.カラーコード作る（色の入力6）
次に、``||logic:でなければ||``の後に、``||variables:変数〜を〜にする||``ブロックを追加して、``||variables:変数||``を **color** にかえます。

```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push(parseFloat(color))
            color = ""
        } else {
            color = 0
        }
    } else {
    	
    }
})
```
## 4.カラーコード作る（色の入力7）
``||variables:変数 color を〜にする|| の右側に ``||text:文字列||``にある ``||text:文字列をつなげる（）（）||``をセットして、１番目の空欄に **color**、2番目の空欄に **c**をそれぞれセットします。

```blocks
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push(parseFloat(color))
            color = ""
        } else {
            color = "" + color + c
        }
    } else {
    	
    }
})
```

## 5. LEDを光らせる @showdialog

ここでは、入力受付モードで入力した値（変数 color）が確定したら、LEDを点灯するプログラムを作ります。

（ボタン操作についての確認）
- これまでに、モードが「入力受付モード」のときにキー入力ができるようになりました。
- 入力した数字から赤、緑、青の値をそれぞれ 0〜255の範囲で配列 rgbに入れました。

次は、Aボタンを押した時に、ここで決めた色で点灯するようにします。


## 4. LEDを光らせる１
``||input:ボタンA〜||`` の中の ``||logic:もし〜なら||``ブロックの**でなければ**のかにある ``||neopixel:strip を点灯する||`` ブロックの 色の部分（**blac**）に、``||neopixel:Neopixel||`` の ``||neopixel:その他||``にある``||neopixel:rgb() () ()||``というブロックを当てはめます。  

```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Red))
    } else {
        strip.showColor(neopixel.rgb(255, 255, 255))
        mode = 0
        basic.showNumber(mode)
    }
})
let strip: neopixel.Strip = null
```

## 4. LEDを光らせる2
新しく追加した ``||neopixel:rgb() () ()||``の空欄に ``||arrays:配列||``にある``||arrays:配列の（）番目||`` をそれぞれ入れて、``||variables:配列||``を``||variables:rgb||``  に変えて、前から **0番目**,**1番目**,**2番目**とします。
また、**mode=0**の中で、テストようにLEDを赤で点灯させて今sたが、**black** に変えて消灯するようにします。

```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
    } else {
        mode = 0
        basic.showNumber(mode)
        strip.showColor(neopixel.rgb(rgb[0], rgb[1], rgb[2]))
    }
})
```


## 4. LEDを光らせる5 テスト @showdialog
ここまでのプログラムです。

```blocks
input.onButtonPressed(Button.A, function () {
    if (mode == 0) {
        mode = 1
        basic.showNumber(mode)
        strip.showColor(neopixel.colors(NeoPixelColors.Black))
    } else {
        mode = 0
        basic.showNumber(mode)
        strip.showColor(neopixel.rgb(rgb[0], rgb[1], rgb[2]))
    }
})
let rgb: number[] = []
let mode = 0
let strip: neopixel.Strip = null
strip = neopixel.create(DigitalPin.P12, 8, NeoPixelMode.RGB)
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
mode = 0
let c = ""
let color = ""
rgb = []
basic.forever(function () {
    if (mode == 1) {
        c = keypad.getKeyString()
        basic.pause(300)
        basic.showString(c)
        if (c == "A") {
            rgb.push(parseFloat(color))
            color = ""
        } else {
            color = "" + color + c
        }
    } else {
    	
    }
})
```
## 5. LEDを光らせる5 テスト
ここまでできたら、micro:bit にダウンロードして実際に動かしてみましょう。
キーパッドは、平らなところにおいて、ゆっくり押さえるようにしてください。



## 🌈 ここまでのプログラムを振り返ろう@showdialog
ここまでで、Aボタンでモードを切り替えて、キーパットから色のrgbの値を指定して、それに対応した色でLEDを光らせるプログラムを作成しました。

少し長いプログラムなので、おおよその処理の流れを図で確認しましょう。

![フローチャート](https://github.com/SKYTREE-1/keypad2/blob/master/images/flow2.jpg?raw=true)

```
## 5. もっと工夫しよう@showdialog

さいごは、keypad と テープLEDを使って、じゆうにあそんでみてください。
色の番号を決める部分は、たとえば、関数にして呼び出すようにすると、コードが見やすくなります。