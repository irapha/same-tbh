# same-tbh
Vim-like macros for bash


Usage:
=====

`$ tbh` to start recording bash commands. `donezo` to stop recording. `$ tbh same` to execute recording.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ ls
hey    other_files
tbh (recording) $ donezo
gottem
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

Default macro is 'dude' so that `$ tbh` == `$ tbh dude` and `$ tbh same` == `$ tbh same dude`.

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
same $ mkdir hi
same $ rm wassup
gottem
```

And you can run the same macro multiple times with different substitutions.

```
$ tbh
tbh (recording) $ mkdir hey
tbh (recording) $ donezo
gottem
$ tbh same except hey=hi,oi
same $ mkdir hi
same $ mkdir oi
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
same $ mkdir hi
same $ rm wassup
same $ mkdir oi
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
