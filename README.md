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
