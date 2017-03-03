<meta charset="utf-8">
<link rel="stylesheet" href="https://tonyday567.github.io/other/lhs.css">
<script type="text/javascript" async
  src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
[tower](https://tonyday567.github.com/tower) [![Build Status](https://travis-ci.org/tonyday567/tower.png)](https://travis-ci.org/tonyday567/tower)
==================================================================================================================================================

A heirarchy of classes for numbers and algebras that combine them: a
numeric tower.

Performance testing, notes and examples can be found in
[tower-dev](https://github.com/tonyday567/tower-dev).

The tower looks something like:

![](https://tonyday567.github.io/other/field-tower.svg)

To use the library:

``` {.sourceCode .literate .haskell}
{-# OPTIONS_GHC -fno-warn-type-defaults #-}
{-# LANGUAGE NoImplicitPrelude #-}
import Tower.Prelude
```

`Tower.Prelude` is a drop-in replacement for `Prelude`. Behind the
scenes, it wraps `Protolude`. `Monoid` and `Semigroup` do not exist in
the tower, to avoid trampling on base.

``` {.sourceCode .literate .haskell}

main :: IO ()
main = do
```

To use literal numbers with tower without a type annotation somewhere,
type defaulting as per Num needs to be applied to Additive.

    :t 1 + 1
    1 + 1 :: (Additive a, Num a) => a

`tower` awaits OverloadedNums to be practical...

``` {.sourceCode .literate .haskell}
  putStrLn $ "1 + 1      = " ++ show (1 + 1 :: Int)
  putStrLn $ "1.0 + 1.0  = " ++ show (1.0 + 1.0 :: Float)
```

`-` as a unary function, and a minus literal eg `-1` is treated
specially by ghc, and also differently to one another.

    :t - 1
    - 1 :: Num a => a
    :t -1
    -1 :: Num t => t

The upshot of this, is that the unary function `-` is not yet hooking
into tower, despite `(-)` and `negate` being hidden in the import of
protolude.

``` {.sourceCode .literate .haskell}
  putStrLn $ "-1         = " ++ show (- 1)
```

The binary function `-` is ok.

``` {.sourceCode .literate .haskell}
  putStrLn $ "1 - 1      = " ++ show (1 - 1 :: Int)
  putStrLn $ "1.0 - 1.0  = " ++ show (1.0 - 1.0 :: Float)
  putStrLn $ "1 * 1      = " ++ show (1 * 1 :: Int)
  putStrLn $ "1.0 * 1.0  = " ++ show (1.0 * 1.0 :: Float)
  putStrLn $ ("1 / 1 is a type error" :: Text)
  putStrLn $ "1.0 / 1.0  = " ++ show (1.0 / 1.0 :: Float)
```

Using `zero` and `one` rather than magical `0`s and `1`s in code is
great practice.

``` {.sourceCode .literate .haskell}
  putStrLn $ "zero       = " ++ show (zero :: Float)
  putStrLn $ "one        = " ++ show (one :: Float)
```

BoundedField ensures that divide by zero works. Having said this,
actually printing one/zero as `Infinity` and -one/zero as `-Infinity`
ain't coming from tower.

``` {.sourceCode .literate .haskell}
  putStrLn $ "one / zero = " ++ show (one / zero :: Float)
  putStrLn $ "(zero / zero) + one = " ++ show (zero / zero + one :: Float)
```

footnotes
---------

To render readme.md etc:

    stack build --test --exec "pandoc -f markdown+lhs -i examples/examples.lhs -t html -o index.html --filter pandoc-include --mathjax" --exec "pandoc -f markdown+lhs -i examples/examples.lhs -t markdown -o readme.md --filter pandoc-include --mathjax" --exec "echo Yah, it succeeded" --file-watch
