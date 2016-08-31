# the tidal suite

a simple `stack.yaml` + `.cabal` file  
to build an isolated haskell environment  
for a specific [Tidal](https://github.com/tidalcycles/Tidal) version (and specific [addons](https://github.com/tidalcycles/tidal-midi))  
and make emacs use it.

## usage

this assumes you:

- have stack installed (in `/usr/bin/stack`)
- have emacs configured for (your main) tidal,  
  that is: you `require 'tidal` somewhere on emacs init.


get the source:

```
git clone https://github.com/lennart/tidal-suite
```

build the environment

```
cd tidal-suite
stack setup && stack build
```

### Test the installation

enter the isolated environment

```
stack repl --ghci-options=-XOverloadedStrings
```

and check whether we can access core tidal features:

```
GHCi, version 8.0.1: http://www.haskell.org/ghc/  :? for help


.
.
.
.
*Main Lib Sound.Tidal.Bjorklund Sound.Tidal.Context ... Sound.Tidal.Transition Sound.Tidal.Utils> :t cpsUtils
cpsUtils :: IO (Double -> IO (), IO Rational)
```

We let us show the type of the core action `cpsUtils` that returns what the pair that is commonly called `cps` and `getNow`. If this works, stack can load tidal.

Leave the stack repl with `CTRL+C` and then `CTRL+D`.

### Running emacs

Start the editor from within the _tidal-suite_ source folder:

```
emacs mod.tidal
```

Accept the question regarding the execution of `.dir-locals.el`  
which sets up the tidal variables  
`tidal-interpreter`  
and `tidal-interpreter-arguments`  
to load stack instead of the default `ghci`

Start up tidal in emacs, usually with `CTRL+C CTRL+S`  
and to check whether you are loading the correct environment, evaluate

```
:show paths
```

via `CTRL+C CTRL+C`, which should list something like:

```
  /home/l/stack/tidal-suite
module import search paths:
  /home/l/stack/tidal-suite/src
  /home/l/stack/tidal-suite/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build/autogen
  /home/l/stack/tidal-suite/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build
  /home/l/stack/tidal-suite/app
  /home/l/stack/tidal-suite/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build/tidal-suite-exe/tidal-suite-exe-tmp
  /home/l/stack/tidal-suite/.stack-work/downloaded/5mXY8JhVRwwt
  /home/l/stack/tidal-suite/.stack-work/downloaded/5mXY8JhVRwwt/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build/autogen
  /home/l/stack/tidal-suite/.stack-work/downloaded/5mXY8JhVRwwt/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build
  /home/l/stack/tidal-suite/.stack-work/downloaded/QTqlNzRoyCnz
  /home/l/stack/tidal-suite/.stack-work/downloaded/QTqlNzRoyCnz/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build/autogen
  /home/l/stack/tidal-suite/.stack-work/downloaded/QTqlNzRoyCnz/.stack-work/dist/x86_64-linux/Cabal-1.24.0.0/build
```

depending on where you placed `tidal-suite`.

### Wow, what's new?

In this case you are looking at the 0.9-dev versions of Tidal  
and tidal-midi, which are not yet released on hackage.

This is a _hopefully_ simple way to test these in combination and isolation.

You can try out the new features of tidal-midi 0.9, as exemplified in `mod.tidal`.

If you are developing on Tidal or tidal-midi or anything related to it, you can use this repository as a foundation to adapt from.

Change the `.cabal` file to your needs and versions of dependencies you require (add them to the library's `build-depends` key) and change the commit hashes or repository urls within `stack.yaml`. Choose different ghc versions, rely on the system ghc if you do not want to build a local one...
