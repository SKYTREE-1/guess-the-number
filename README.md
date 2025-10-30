
> このページを開く [https://skytree-1.github.io/guess-the-number/](https://skytree-1.github.io/guess-the-number/)
## 拡張機能として使用

このリポジトリは、MakeCode で **拡張機能** として追加できます。

* [https://makecode.microbit.org/](https://makecode.microbit.org/) を開く
* **新しいプロジェクト** をクリックしてください
* ギアボタンメニューの中にある **拡張機能** をクリックしてください
* **https://github.com/skytree-1/guess-the-number** を検索してインポートします。

## このプロジェクトを編集します

MakeCode でこのリポジトリを編集します。

* [https://makecode.microbit.org/](https://makecode.microbit.org/) を開く
* **読み込む** をクリックし、 **URLから読み込む...** をクリックしてください
* **https://github.com/skytree-1/guess-the-number** を貼り付けてインポートをクリックしてください


```blocks
input.onButtonPressed(Button.A, function () {
    basic.showNumber(custom.baz())
})
input.onButtonPressed(Button.B, function () {
    basic.showNumber(custom.bar())
})
```



#### メタデータ (検索、レンダリングに使用)

* for PXT/microbit
<script src="https://cdn.jsdelivr.net/gh/jp-rad/pxt-ubit-extension@0.5.0/.github/statics/gh-pages-embed.js"></script>
<script>makeCodeRender("{{ site.makecode.home_url }}", [ "custom=github:jp-rad/pxt-ubit-extension", ]);</script>



