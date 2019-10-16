<!--
# Buttons
-->
# ボタン

<!--
#### Follow along in [the online editor](https://elm-lang.org/examples/buttons).
-->

---
#### [オンラインエディタ](https://elm-lang.org/examples/buttons)で試してください。
---


<!--
Our first example is a counter that can be incremented or decremented. I find that it can be helpful to see the entire program in one place, so here it is! We will break it down afterwards.
-->
最初の例はインクリメントやデクリメントを行うカウンタの作成です。プログラムすべてを一箇所にまとめて置けば読みやすいでしょう、用意しておきました！あとでひとつひとつ詳細をみていきます。

```elm
import Browser
import Html exposing (Html, button, div, text)
import Html.Events exposing (onClick)


main =
  Browser.sandbox { init = init, update = update, view = view }


-- MODEL

type alias Model = Int

init : Model
init =
  0


-- UPDATE

type Msg = Increment | Decrement

update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1


-- VIEW

view : Model -> Html Msg
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ]
    , div [] [ text (String.fromInt model) ]
    , button [ onClick Increment ] [ text "+" ]
    ]
```

<!--
That's everything!
-->
これで全部です！

<!--
> **Note:** This section has `type` and `type alias` declarations. You can read all about these in the upcoming section on [types](/types/index.html). You do not *need* to deeply understand that stuff now, but you are free to jump ahead if it helps.
-->
> **Note:** この章は `type` と `type alias` 宣言を含みます。それら詳細は次章である [型](/types/index.html) で読むことができます。この時点で深く理解しておく*必要はありません*が、読むことで理解の手助けになる場合はご自由に読んで下さい。

<!--
When writing this program from scratch, I always start by taking a guess at the model. To make a counter, we at least need to keep track of a number that is going up and down. So let's just start with that!
-->
カウンタプログラムを白紙から書くとき、私はいつもモデルを考えることから始めます。カウンタを作るには、最低でも増えたり減ったりする数字を記録する必要があります。それではモデルに関するところだけ書き始めましょう！

```elm
type alias Model = Int
```

<!--
Now that we have a model, we need to define how it changes over time. I always start my `UPDATE` section by defining a set of messages that we will get from the UI:
-->
モデルができたら、次は時が経つとどのように数字が変わるのか定義していきます。私はUIから受け取るメッセージ群を定義することで書ける `UPDATE` の部分からいつも始めます。

```elm
type Msg = Increment | Decrement
```

<!--
I definitely know the user will be able to increment and decrement the counter. The `Msg` type describes these capabilities as *data*. Important! From there, the `update` function just describes what to do when you receive one of these messages.
-->
上記のコードを見ると、ユーザがカウンタをインクリメントやデクリメントできることが明確に分かります。`Msg`型はそういった機能を*データ*として記述します。これは重要なことです！それでは、メッセージを受信したとき何をすべきかを `update` 関数にそのまま記述しましょう。


```elm
update : Msg -> Model -> Model
update msg model =
  case msg of
    Increment ->
      model + 1

    Decrement ->
      model - 1
```

<!--
If you get an `Increment` message, you increment the model. If you get a `Decrement` message, you decrement the model. Pretty straight-forward stuff.
-->
`Increment`メッセージを受け取ったらモデルをインクリメントし、`Decrement`メッセージを受け取ったらデクリメントします。非常に簡潔な関数です。

<!--
Okay, so that's all good, but how do we actually make some HTML and show it on screen? Elm has a library called `elm/html` that gives you full access to HTML5 as normal Elm functions:
-->
よし、すべて順調ですね。次の問題ですが、どうやってHTMLを作成しそれを画面で表示しましょうか？ Elmは`elm/html`というライブラリを持っており、これを使うと通常のElm関数としてHTML5のすべてを書き表せます:

```elm
view : Model -> Html Msg
view model =
  div []
    [ button [ onClick Decrement ] [ text "-" ]
    , div [] [ text (String.fromInt model) ]
    , button [ onClick Increment ] [ text "+" ]
    ]
```

<!--
One thing to notice is that our `view` function is producing a `Html Msg` value. This means that it is a chunk of HTML that can produce `Msg` values. And when you look at the definition, you see the `onClick` attributes are set to give out `Increment` and `Decrement` values. These will get fed directly into our `update` function, driving our whole app forward.
-->
注目すべき点は、`view`関数が`Html Msg`値を返すことです。`Html Msg`は`Msg`の値を生み出しうるHTMLのかたまりだということを意味しています。`view`関数の定義を見てもらうと、`Increment`や`Decrement`のメッセージを生み出すために`onClick`属性が設定されているのが分かるでしょう。これらは`update`関数にそのまま入力され、アプリ全体を動作させます。

<!--
Another thing to notice is that `div` and `button` are just normal Elm functions. These functions take (1) a list of attributes and (2) a list of child nodes. It is just HTML with slightly different syntax. Instead of having `<` and `>` everywhere, we have `[` and `]`. We have found that folks who can read HTML have a pretty easy time learning to read this variation. Okay, but why not have it be *exactly* like HTML? **Since we are using normal Elm functions, we have the full power of the Elm programming language to help us build our views!** We can refactor repetitive code out into functions. We can put helpers in modules and import them just like any other code. We can use the same testing frameworks and libraries as any other Elm code. Everything that is nice about programming in Elm is 100% available to help you with your view. No need for a hacked together templating language!
-->
もうひとつの注目すべき点は、`div`と`button`は単なる通常のElm関数であることです。これらの関数は (1) 属性のリストと (2) 子ノードのリストを引数に取ります。HTMLのシンタックスとほんの少し異なるだけです。そこかしこに`<`と`>`を置く代わりにElmでは`[`と`]`を置きます。HTMLを読めるあなた方にとっては、この変化に慣れることは非常に容易でしょう。でも、どうしてHTMLと*まったく*同じ外観にしないのでしょう？**その理由は、Elm関数を使うことでviewを組み立てるときにElm言語の力を最大限に引き出せるからです！**重複したコードを関数にまとめられます。さらに、それを補助関数としてモジュールに追加し、他のview以外の関数と同じようにインポートして使えます。また、他のElmコードと同じテストフレームワークやライブラリを使えます。Elmが持つプログラミングの利点すべてをviewで100%活用できるのです。もしもHTMLとまったく同じ外観だったなら、重複したコードをまとめたりするためにはPugやMustacheといったテンプレートエンジンの機能を使ってどうにかする必要があったでしょう。しかし実際にはそんな苦労をする必要はありません、Elmの関数で表現できるので！

<!--
There is also something a bit deeper going on here. **The view code is entirely declarative**. We take in a `Model` and produce some `Html`. That is it. There is no need to mutate the DOM manually. Elm takes care of that behind the scenes. This gives Elm [much more leeway to make optimizations](https://elm-lang.org/blog/blazing-fast-html) and ends up making rendering *faster* overall. So you write less code and the code runs faster. The best kind of abstraction!
-->
もう少し掘り下げます。**view部分のコードはすべて宣言的に書かれます。**`Model`を受け取り`Html`を生成する、それだけです。書き手がDOMに直接触れる必要もありません。Elmはそれを隠してうまくやってくれます。こうすることでElmに[最適化する裁量を十二分に与えることができ](https://elm-lang.org/blog/blazing-fast-html)、その結果、全体のレンダリングを*速く*することになります。少ないコードを書くだけでそのコードは速く動作するのです。抽象化によるすごい効果です！

<!--
This pattern is the essence of The Elm Architecture. Every example we see from now on will be a slight variation on this basic pattern: `Model`, `update`, `view`.
-->
このパターンはThe Elm Architectureの本質です。これから示すすべての例はこの基本パターンである`Model`, `update`, `view`を少し変えたものです。


<!--
> **Exercise:** One cool thing about The Elm Architecture is that it is super easy to extend as our product requirements change. Say your product manager has come up with this amazing "reset" feature. A new button that will reset the counter to zero.
>
> To add the feature you come back to the `Msg` type and add another possibility: `Reset`. You then move on to the `update` function and describe what happens when you get that message. Finally you add a button in your view.
>
> See if you can implement the "reset" feature!
-->
> **練習問題:** The Elm Architectureが優れていることの一つに、プロダクトに変更が必要な場合はとても簡単に拡張できることが挙げられます。あなたのプロダクトマネージャが"reset"機能を思い付いたとしましょう。新しいボタンはカウンタを0にリセットします。
>
> この機能を追加するには`Msg`型の実装部分にもどってもう一つの可能性の`Reset`を追加しましょう。メッセージを受け取ったときに何をすべきかを`update`関数に記述して下さい。最後はviewにボタンを追加して下さい。
>
> それでは、"reset"機能を実装してみましょう！
