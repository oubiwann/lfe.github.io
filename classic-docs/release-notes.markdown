---
layout: docs
---

# LFE Documentation

## New features in LFE 0.2

- The really *BIG* change is that LFE is now Lisp-2 like Common Lisp,
  i.e. functions and variables reside in different name spaces,
  instead being a Lisp-1 like Scheme where they reside in the same
  name space. The reason for this change is that the ErlangVM does
  keep variables and functions separate and while Core Erlang tries to
  hide this fact it does not fully succeed. In fact, it is actually
  impossible to do this given Erlang's property of being able to have
  many functions of the same name but with different arites.

  While this is not as elegant and forces the use of funcall to call
  functions bound to variables it works better.

  It is not an irrevocable change but I would need really convincing
  arguments to change it back.

- Being a Lisp-2 has introduced some new core forms to handle them:
  ```flet```, ```flet*```, ```fletrec``` and ```funcall```. ```Letrec```
  has been removed.

- The handling of macros has been cleaned up.

- Schemes, R5RS, handling of ellipsis '...' in syntax-rules has been
  added. This really simplifies writing some macros in a elegant way.

- The interpreter, lfe_eval, can now handle the complete language.

- In patterns both tuples and binaries/bitstrings use the general
  constructor form, the constant form now only matches a literal tuple
  or binary. For example:

      (tuple 'ok a b)    - is eqivalent to {ok,A,B}
      #('ok a b)         - is eqivalent to {[quote,ok],a,b}
      (binary (f float (size 32)) (rest binary))
                - is eqivalent to <<F:32/float,Rest:binary>>

  Even though this may be longer and, in some cases, more difficult to
  "see" I prefer it as it is consistent.

- Patterns can now have aliases, (= pat-1 pat-2). These are checked in
  the linter and non-matching patterns are classed as an error. In
  future releases they should become warnings.

- There is now an LFE shell which evaluates expressions and prints the
  results. It is still a bit primitive and doesn't use processes as it
  should in the same manner as the standard Erlang shell does. But it
  does have one very nice feature, you can slurp in a source file and
  run evaluate all the functions in the shell. Any macros defined in
  the file are also available.

  It is not yet possible to define functions/macros in the shell but
  that should use soon be possible. You should also then be able to do
  regurgitate which would write all the definitions out to a file.

- Running a shell other than the standard erlang one is a bit
  difficult so I have included a patched version of user_drv.erl from
  the standard kernel app which allows you to start specific shells,
  both local and remote. You can either put it into the distribution
  or leave it where you run the LFE system and it will be picked
  up. As far as I can see it is "safe" to use.

- There are two versions of the interpreter, one written in Erlang,
  the standard, and a compatible one written in LFE. Lfe_eval.lfe is
  not written in a consistent manner and can be seen as an example of
  the various styles of LFE programming, for example using
  match-lambda or lambda/case.

- As before there are a number of test files included as example code
  in lieu of better documentation. They are also useful to see the
  Core code generated by the LFE compiler, just add an option
  [dcore]. N.B. Not all the test files compile, but this is on purpose
  to test linter.

- There is now a lisp prettyprinter in lfe_io. Unfortunately the io
  functions in lfe_io are not always obviously named from a lisp
  viewpoint.
