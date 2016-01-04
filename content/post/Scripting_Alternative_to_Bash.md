+++
date = "2013-10-25T12:00:00+08:00"
title = "Scripting Alternative to Bash"

+++

I really don't like to write bash script. I prefer use python as my shell language, but I have to admit that in some cases, bash script can be simpler than python. I found the discussion [Are there any languages that compile to Bash](http://stackoverflow.com/questions/10239235/are-there-any-languages-that-compile-to-bash)?

Someone mention [Plumbum](http://plumbum.readthedocs.org/en/latest/index.html) for python and [ShellJS](https://github.com/arturadib/shelljs) for nodejs both reimplement some unix tools. I tried both, here's what I learned.

* Plumbum

    > overriding many python operator `| < >` …etc. give these operators new meaning such that `>` or `<` for redirection.

* ShellJS

    > javascript syntax is not as simple as python, but there are coffee script/livescript which provide much elegant syntax, make this library worth using.

there is a third option: `ipython`, I had read the book

![](http://it-ebooks.info/images/ebooks/3/python_for_unix_and_linux_system_administration.jpg)

ipython is really amazing, I use it as my default pytohn REPL, this book give me lot's of way to write a bash like script in python way, oh… not exactly pure python, ipython invent some of it's own syntax.
Here are some example

    !ls: execute ls
    !ls | grep config: ls with filter
    %rehash: hash command in PATH. make unix command can be use inside ipython REPL.

    (ipython) user = 'root'
    (ipython) process = 'bash'
    (ipython) listofitem = !ps aux | grep $user | grep $process
