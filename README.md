# same-tbh
Vim-like macros for bash


Usage:
=====

`$ tbh` to start recording bash commands. `donezo` to stop recording.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ ls
hey    other_files
tbh (recording) $ donezo
gottem
```

`$ tbh same` to execute recording.

```
$ rm -rf hey
$ tbh same
same $ mkdir hey
same $ ls
hey    other_files
gottem
```


Named Macros
============

You can name macros by doing `$ tbh macro_name`. Then execute it with `$ tbh same macro_name`.
You can also execute saved macros within macros (but you can't record within a recording).

Default macro is 'dude' so that `$ tbh` == `$ tbh dude`, `$ tbh same` == `$ tbh same dude` and `$ tbh wtf` == `$ tbh wtf dude`.

To look up available macros, do 
```
$ tbh wtf
macros: dude, bro
$ tbh wtf dude
dude: ['mkdir rad']
```

To delete a macro:
```
$ tbh fuk dude
dude got rekt
```


Variables
=========

This is what makes this useful. You can change anything in the saved commands with the keyword `except`.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ donezo
gottem
$ tbh same except hey=hi
same $ mkdir hi
gottem
```

You can make multiple substitutions too.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ rm sup
tbh (recording) $ donezo
gottem
$ tbh same except hey=hi sup=wassup
same $ mkdir hi             <- hey became hi
same $ rm wassup            <- sup became wassup
gottem
```

And you can run the same macro multiple times with different substitutions.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ donezo
gottem
$ tbh same except hey=hi,oi
same $ mkdir hi       <- macro runs once with hey=hi
same $ mkdir oi       <- macro runs second time, with hey=oi
gottem
```

If you make multiple substitutions, they will be matched together.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ rm sup
tbh (recording) $ donezo
gottem
$ tbh same except hey=hi,oi sup=wassup,bro
same $ mkdir hi         <- macro runs once, with hey=hi and sup=wassup
same $ rm wassup
same $ mkdir oi         <- macro runs second time, with hey=oi and sup=bro
same $ rm bro
gottem
```

Finally, if you ever want to execute the commands in reverse order:

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ rm sup
tbh (recording) $ donezo
gottem
$ tbh same except opposite
same $ rm sup
same $ mkdir hey
gottem
```

(and of course, you can combine any and all of these).


Cool stuff
==========

If you make a macro that runs the macro, then run the macro, you go in an infinite loop.

```
$ tbh meh
tbh (recording) $ tbh same meh
nothing to same
tbh (recording) $ donezo
gottem
$ tbh same meh
same $ tbh same meh
same $ tbh same meh
same $ tbh same meh
same $ tbh same meh
same $ tbh same meh
same $ tbh same meh
same $ tbh same meh
^C
Traceback (most recent call last):
    (...)
KeyboardInterrupt
gottem
gottem
gottem
gottem
gottem
gottem
gottem
$ (whew...) 
```

Excepts will not propagate down macros within macros automatically. 

```
$ tbh hey
tbh (recording) $ mkdir bro
tbh (recording) $ donezo
gottem
$ tbh wassup
tbh (recording) $ rm -rf bro
tbh (recording) $ tbh same hey
same $ mkdir bro
gottem
tbh (recording) $ donezo
gottem
$ tbh same wassup except bro=brah
same $ rm -rf brah
same $ tbh same hey
same $ mkdir bro          <- substitution did not happen.
mkdir: bro: File exists
gottem
gottem
```

If you want the propagation to happen, you have to make the inner macro have its own except.

```
$ tbh hey
tbh (recording) $ mkdir something
tbh (recording) $ donezo
gottem
$ tbh wassup
tbh (recording) $ rm -rf bro
tbh (recording) $ tbh same hey except something=bro
same $ mkdir bro
gottem
tbh (recording) $ donezo
gottem
$ tbh same wassup except bro=brah
same $ rm -rf brah
same $ tbh same hey except something=brah
same $ mkdir brah        <- substitution did happen.
gottem
gottem
```

Let me know if you can do cooler stuff.
