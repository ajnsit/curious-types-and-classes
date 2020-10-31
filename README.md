# curious-types-and-classes
Curious types and type classes found in the wild, and where to find them (i.e. where they are used).


## Typeclasses

### ShiftMap

```purescript
class ShiftMap s t where
  shiftMap :: forall a. (forall b. (a -> b) -> s b -> s b) -> t a -> t a
```

Used in the Concur UI framework (https://github.com/purescript-concur/purescript-concur-core/blob/master/src/Control/ShiftMap.purs#L14) as a way to avoid monad-control. There are a bunch of instances. Related to natural transformations. Unknown laws. The `s`, `t`, `a`, `b` formulation is reminiscent of lenses. Haven't seen this used anywhere else.

#### Explanation

It's a more flexible version of a mapping of natural-transformations `(s ~> s) -> (t ~> t)`. The idea behind the earlier version was to associate structures where a transformation on one can be translated into a transformation into the other. However there was a need for a more powerful version where the transformation had some idea of the values being transformed. Hence this new formulation. It is like a natural transformation mapping that allows not just transforming, but also adding structure.

### Royal Monad

```haskell
class RoyalMonad m n p r where
  royalReturn :: r a -> m a
  royalBind   :: m a -> (r a -> n b) -> p b
```
From -
https://github.com/atzeus/RoyalMonad/blob/f93b40e5f172358db5d500ba546d154cdf9f3368/Control/Monad/Royal.hs#L40

They generalise both Relative monads and poly monads. I'm yet to find a usecase for any of this. 

#### Relative Monads -

```haskell
class RelativeMonad m r where
  relReturn :: r a -> m a
  relBind   :: m a -> (r a -> m b) -> m b
```

There's a potential use for relative monads here - https://github.com/ajnsit/monadbi/issues/1#issuecomment-139986776
Quoting -

> I think it may be equivalent to what you had for `monadbi` in the sense that you can convert between instances of the two type classes.  

> The issue with `StateT` I think is clearer with `rBind` - if some state is in `m a` but not in `r a` then the laws require that `rBind` appropriately propagate the state to the result of the rBind, similar to an ordinary bind.

> But, the main code organisation we're emphasizing is a little different to monad transformers with lifts, we're instead organising our code by defining type classes of operations that are available on "all monads `m` that are relative to a particular type `R`, i.e., we have an instance of `RelMonad R m`".  Then, we choose a specific type `N` for each of our concrete monads and implement `RelMonad R N` instances, i.e., `rReturn` and `rBind`, generally directly, since they are pretty simple, but we can also compose simpler `RelMonad` instances. 

#### Poly Monads -

```haskell
class PolyMonad m n p where
  relReturn :: a -> m a
  relBind   :: m a -> (a -> n b) -> p b
```
