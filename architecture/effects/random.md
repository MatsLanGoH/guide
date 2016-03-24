# Random


We are about to make an app that "rolls dice", producing a random number between 1 and 6.

When I write code with effects, I usually break it into two phases. Phase one is about getting something on screen, just doing the bare minimum to have something to work from. Phase two is filling in details, gradually approaching the actual goal. We will use this process here too.


## Phase One - The Bare Minimum

As always, you start out by guessing at what your `Model` should be:

```elm
type alias Model =
  { dieFace : Int
  }
```

For now we will just track `dieFace` as an integer between 1 and 6. Then I would quickly sketch out the `view` function because it seems like the easiest next step.

```elm
view : Model -> Html Msg
view model =
  h1 [ onClick Roll ] [ text model.dieFace ]
```

So this is typical. Same stuff we have been doing with the user input examples of The Elm Architecture. When you click our `<h1>` it is going to produce a `Roll` messages, so I guess it is time to take a first pass at the `update` function as well.

```elm
type Msg = Roll

update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Roll ->
      (model, Cmd.none)
```

Now the `update` function has the same overall shape as before, but the return type is a bit different. Instead of just giving back a `Model`, it produces both a `Model` and a command. The idea is: **we still want to step the model forward, but we also want to do some stuff.** In our case, we want to ask Elm to give us a random value. For now, I just fill it in with [`Cmd.none`](TODO) which means "I have no commands, do nothing." We will fill this in with the good stuff in phase two.

Finally, I would create an `init` value like this:

```elm
init : (Model, Cmd Msg)
init =
  (Model 1, Cmd.none)
```

Here we specify both the initial model and some commands we'd like to run immediately when the app starts. This is exactly the kind of stuff that `update` is producing now too.

At this point, it is possible to wire it all up and take a look. You can click the `<h1>`, but nothing happens. Let's fix that!


## Phase Two - Adding the Cool Stuff

The obvious thing missing right now is the randomness! When the user clicks a button we want to command Elm to reach into its internal random number generator and give us a number between 1 and 6. The first step I would take towards that goal would be adding a new kind of message:

```elm
type Msg
  = Roll
  | NewFace Int
```

We still have `Roll` from before, but now we add `NewFace` for when Elm hands us our new random number. That is enough to start filling in `update`:

```elm
update : Msg -> Model -> (Model, Cmd Msg)
update msg model =
  case msg of
    Roll ->
      (model, Random.request NewFace (Random.int 1 6))

    NewFace newFace ->
      (Model newFace, Cmd.none)
```

There are two new things here. **First**, there is now a branch for `NewFace` messages. When a `NewFace` comes in, we just step the model forward and do nothing. **Second**, we have added a real command to the `Roll` branch. This uses a couple functions from [the `Random` library](TODO). Most important is `Random.request`:

```elm
Random.request : (a -> msg) -> Random.Generator a -> Cmd msg
```

This function takes two arguments. The first is a function to tag random values. In our case we want to use `NewFace : Int -> Msg` to turn the random number into a message for our `update` function. The second argument is a "generator" which is like a recipe for producing certain types of random values. You can have generators for simple types like `Int` or `Float` or `Bool`, but also for fancy types like big custom records with lots of fields. In this case, we use one of the simplest generators:

```elm
Random.int : Int -> Int -> Random.Generator Int
```

You provide a lower and upper bound on the integer, and now you have a generator that produces integers in that range!

That is it! Now we can click the 

> **Exercises:** Here are some that build on stuff that has already been introduced:
> 
>   - Instead of showing a number, show the die face as an image.
>   - Add a second die and have them both roll at the same time.
>
> And here are some that require new skills:
> 
>   - Instead of showing an image of a die face, use the `elm-lang/svg` library to draw it yourself.
>   - After you have learned about tasks and animation, have the die flip around randomly before they settle on a final value.

