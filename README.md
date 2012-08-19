Snaplet Fay
===========

Snaplet Fay integrates [Snap](http://www.snapframework.com) with
[Fay](http://www.fay-lang.org). Snap is a
[Haskell](http://www.haskell.org) web framework and Fay is a compiler
from a proper subset of Haskell to JavaScript. Snaplet Fay integrates
them nicely with each other allowing automatic (re)compilation of Fay
source files. Snap provides this for both static content and haskell
sources preventing web server restarts and here we add Fay to this
list as well. This lets us write both front and back-end code in Haskell.


Features
--------

* Compile and serve fay files automatically, no need to restart the
  snap server.
* Uses Fay's pretty print option (js-beautify) to create JS files that
  are easier to debug.
* Writes JS to disk to allow reading the generated source.
* Share Fay source files between Haskell and Fay.
* Automatically encode/decode shared records by using the fayax functions.


Installation
------------

You will need Haskell. The simplest way to get up and running with is
to install
[The Haskell Platform](http://hackage.haskell.org/platform/).

Everything else is available on hackage:
```
cabal install snaplet-fay
```


Example Usage
-------------

Site.hs:
```
import Snap.Snaplet.Fay

routes = [..., ("/fay", with fay fayServe)]

app :: SnapletInit App App
app = makeSnaplet "app" "A snaplet example application." Nothing $ do
  fay' <- nestSnaplet "fay" fay initFay
  return $ App { _fay = fay' }
```

Application.hs:
```
import Snap.Snaplet.Fay

data App = App { _fay :: Snaplet Fay }

makeLens ''App
```

Run your application now.

A snaplet config file will be generated at snaplets/fay/devel.cfg the
first time your application initializes the snaplet. The defaults are
the recommended ones for development.

Place your Fay .hs files in snaplets/fay/src. Note that a default
devel.cfg will not be created if you have already created the fay
directory, if this happens to you move snaplets/fay, start your
application, and then move the files back into snaplets/fay.

Any requests to the specified directory (in this case /fay/) will
compile the appropriate Fay file and serve it.

Development Status
------------------

Snaplet Fay is functioning and will be updated to keep up with both
Snap and Fay.


Contributions
-----------

Fork on!

Any enhancements are welcome.

The github master usually requires the latest fay master, available at
[faylang/fay](https://github.com/faylang/fay/).


To run the tests, do:
```
cabal configure -ftest
cabal build
./dist/build/test/test
```


Contact
-------

File an issue, e-mail or visit `#fay @ irc.freenode.net`.
