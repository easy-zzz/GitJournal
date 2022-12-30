---
link: "https://elv.sh/learn/tour.html"
---

# [Quick Tour - Elvish Shell](https://elv.sh/learn/tour.html)  

Добро пожаловать в краткий обзор эльфийского языка. Этот тур работает лучше всего, если вы использовали другая оболочка или язык программирования перед. 

Если вы знакомы с традиционными оболочками, такими как Bash, разделы [основной язык оболочки](https://elv.sh/learn/tour.html#basic-shell-language) а также [shell scripting commands](https://elv.sh/learn/tour.html#shell-scripting-commands) может помочь вам «перевести» ваши знания на Elvish. 

Если вы в основном заинтересованы в интерактивном использовании эльфийского языка, перейдите непосредственно к [интерактивным функциям](https://elv.sh/learn/tour.html#interactive-features ).  


P0
## 2.1. Сравнение с bash[](https://elv.sh/learn/tour.html#comparison-with-bash)  

Если вы знакомы с bash, в следующей таблице показано некоторое (приблизительное) соответствие между синтаксисом Elvish и bash:

## 2.2. Barewords[](https://elv.sh/learn/tour.html#barewords)  

Как и традиционные оболочки, слова без кавычек, не содержащие специальных символов трактуются как строки (такие слова называются **barewords**): 

``` sh
ls/bin
```  

Это одна из самых отличительных синтаксических особенностей оболочек; без оболочки языки программирования обычно обрабатывают слова без кавычек как имена функций и переменные. 

Прочитайте языковую справку на
[barewords](https://elv.sh/ref/language.html#bareword ), чтобы узнать больше.

## 2.3. Single-quoted strings[](https://elv.sh/learn/tour.html#single-quoted-strings)  

Как и традиционные оболочки, строки в одинарных кавычках ничего не расширяют; каждый персонаж  
представляет себя (кроме самой одинарной кавычки):  

```  
~> echo'hello\worldhello\world$  
```  

Like [Plan 9 rc](http://doc.cat-v.org/plan_9/4th_edition/papers/rc), or zsh with  
the `RC_QUOTES` option turned on, the single quote itself can be written by  
doubling it  

```  
~> echo'it''s good'it's good  
```  

Read the language reference  
on[single-quoted strings](https://elv.sh/ref/language.html#single-quoted-string)  
to learn more.  

## 2.4. Double-quoted strings[](https://elv.sh/learn/tour.html#double-quoted-strings)  

Like many non-shell programming languages and `'` in bash, double-quoted  
strings support C-like escape sequences (e.g. `\n` for newline):  

```  
~> echo"foo\nbar"foobar  
```  

Unlike traditional shells, Elvish does **not** support interpolation inside  
double-quoted strings. Instead, you can just write multiple words together, and  
they will be concatenated:  

```  
~> varx=foo~> put'x is '$x▶ 'x is foo'  
```  

Read the language reference  
on[double-quoted strings](https://elv.sh/ref/language.html#double-quoted-string)  
to learn more.  

Comments start with `#` and extend to the end of the line:  

```  
~> echofoo # this is a commentfoo  
```  

## 2.6. Line continuation[](https://elv.sh/learn/tour.html#line-continuation)  

Line continuation in Elvish uses `^` instead of `\`:  

```  
~> echofoo ^barfoo bar  
```  

Unlike traditional shells, line continuation is treated as whitespace. In  
Elvish, the following code outputs `foo bar`:  

```  
echofoo^bar  
```  

However, in bash, the following code outputs `foobar`:  

```  
echo foo\  
bar  

```  

## 2.7. Brace expansion[](https://elv.sh/learn/tour.html#brace-expansion)  

Brace expansions in Elvish work like in traditional shells, but use spaces  
instead of commas:  

```  
~> echo{foobar}.txtfoo.txt bar.txt  
```  

The opening brace `{`**must not** be followed by a whitespace, to disambiguate  
from [lambdas](https://elv.sh/learn/tour.html#lambdas).  

**Note**: commas might still work as a separator in Elvish’s brace expansions,  
but it will eventually be deprecated and removed soon.  

Read the language reference on  
[braced lists](https://elv.sh/ref/language.html#braced-list)to learn more.  

## 2.8. Wildcards[](https://elv.sh/learn/tour.html#wildcards)  

The basic wildcard characters, `*` and `?`, work like in traditional shells:  

```  
~> lsbar.ch  d1  d2  d3  foo.c  foo.h  lorem.go  lorem.txt~> echo*.?foo.c foo.h  
```  

Elvish also supports `**`, which matches multiple path components:  

```  
~> find.-name'*.go'./d1/a.go./d2/b.go./lorem.go./d3/d4/c.go~> echo**.god1/a.go d2/b.go d3/d4/c.go lorem.go  
```  

Character classes are a bit more verbose in Elvish. First, a character set is  
written like `[set:ch]`, instead of just `[ch]`. Second, they don’t appear on  
their own, but as a suffix to `?`. For example, to match files ending in  
either`.c` or `.h`, use:  

```  
~> echo*.?[set:ch]foo.c foo.h  
```  

A character set suffix can also be applied to `*`. For example, to match files  
who extension only contains `c` and `h`:  

```  
~> echo*.*[set:ch]bar.ch foo.c foo.h  
```  

Read the language reference  
on[wildcard expansion](https://elv.sh/ref/language.html#wildcard-expansion) to  
learn more.  

## 2.9. Tilde expansion[](https://elv.sh/learn/tour.html#tilde-expansion)  

Tilde expansion works likes in traditional shells. Assuming that the home  
directory of the current user is `/home/me`, and the home directory of `elf`  
is`/home/elf`:  

```  
~> echo~/foo/home/me/foo~> echo~elf/foo/home/elf/foo  
```  

Read the language reference  
on[tilde expansion](https://elv.sh/ref/language.html#tilde-expansion) to learn  
more.  

## 2.10. Setting variables[](https://elv.sh/learn/tour.html#setting-variables)  

Variables are declared with the `var` command, and set with the `set` command:  

```  
~> varfoo=bar~> echo$foobar~> setfoo=quux~> echo$fooquux  
```  

The spaces around `=` are mandatory.  

Unlike traditional shells, variables must be declared before they can be set;  
setting an undeclared variable results in an error.  

Like traditional shells, Elvish supports setting a variable temporarily for the  
duration of a command, by prefixing the command with `foo=bar`. For example:  

```  
~> varfoo=original~> fnf{echo$foo}~> foo=temporaryftemporary~> echo$foooriginal  
```  

Read the language reference on  
[the `var` command](https://elv.sh/ref/language.html#var),[the `set` command](https://elv.sh/ref/language.html#set)  
and[temporary assignments](https://elv.sh/ref/language.html#temporary-assignment)  
to learn more.  

## 2.11. Using variables[](https://elv.sh/learn/tour.html#using-variables)  

Like traditional shells, using the value of a variable requires the `
 prefix.  

```  
~> varfoo=bar~> echo$foobar  
```  

Unlike traditional shells, variables must be declared before being used; if  
the`foo` variable wasn’t declared with `var` first, `echo $foo` results in an  
error.  

Elvish does not perform `$IFS` splitting on variables, so `$foo` always  
evaluates to one value, even if it contains whitespaces and newlines. You never  
need to write `echo "$foo"` again. (And in  
fact,[double-quoted strings](https://elv.sh/ref/language.html#double-quoted-string)  
do not support interpolation).  

Also unlike traditional shells, environment variables in Elvish live in a  
separate `E:` namespace:  

```  
~> echo$E:HOME/home/elf  
```  

Read the language reference on  
[variables](https://elv.sh/ref/language.html#variable),[variable use](https://elv.sh/ref/language.html#variable-use)  
and[special namespaces](https://elv.sh/ref/language.html#special-namespaces) to  
learn more.  

## 2.12. Redirections[](https://elv.sh/learn/tour.html#redirections)  

Redirections in Elvish work like in traditional shells. For example, to save the  
first 10 lines of `a.txt` to `a1.txt`:  

```  
~> head-n10<a.txt>a1.txt  
```  

Read the language reference on  
[redirections](https://elv.sh/ref/language.html#redirection)to learn more.  

## 2.13. Byte pipelines[](https://elv.sh/learn/tour.html#byte-pipelines)  

UNIX pipelines in Elvish (called **byte pipelines**, to distinguish  
from[value pipelines](https://elv.sh/learn/tour.html#value-pipelines)) work like  
in traditional shells. For example, to find occurrences of `x` in the first 4  
lines of `a.txt`:  

```  
~> cata.txtfoobarxloremquuxluxnox~> head-n4a.txt|grepxbarxquux  
```  

Read the language reference on  
[pipelines](https://elv.sh/ref/language.html#pipeline) to learn more.  

## 2.14. Output capture[](https://elv.sh/learn/tour.html#output-capture)  

Output of commands can be captured and used as values with `()`. For example,  
the following command shows details of the `elvish` binary:  

```  
~> ls-l(whichelvish)-rwxr-xr-x 1 xiaq users 7813495 Mar  2 21:32 /home/xiaq/go/bin/elvish  
```  

**Note**: the same feature is usually known as _command substitution_ in  
traditonal shells.  

Unlike traditional shells, Elvish only splits the output on newlines, not any  
other whitespace characters.  

Read the language reference  
on[output capture](https://elv.sh/ref/language.html#output-capture) to learn  
more.  

## 2.15. Background jobs[](https://elv.sh/learn/tour.html#background-jobs)  

Add `&` to the end of a pipeline to make it run in the background, similar to  
traditional shells:  

```  
~> echofoo&foojob echo foo & finished  
```  

Unlike traditional shells, the `&` character does not serve to separate  
commands. In bash you can write `echo foo & echo bar`; in Elvish you still need  
to terminate the first command with `;` or newline: `echo foo &; echo bar`.  

Read the language reference  
on[background pipelines](https://elv.sh/ref/language.html#background-pipeline)  
to learn more.  

## 2.16. Command sequence[](https://elv.sh/learn/tour.html#command-sequence)  

Join commands with a `;` or newline to run them sequentially (insert a newline  
with Alt-Enter):  

```  
~> echoa;echobab~> echoaechobab  
```  

In Elvish, when a command fails (e.g. when an external command exits with a  
non-zero status), execution gets terminated.  

```  
~> echobefore;false;echoafterbeforeException: false exited with 1[tty 2], line 1: echo before; false; echo after  
```  

In this aspect, Elvish’s behavior is similar to joining all commands with `&&`or  
setting `set -e` in traditional shells:  

Building on a core of familiar shell-like syntax, the Elvish language  
incorporates many advanced features that make it a modern dynamic programming  
language.  

## 3.1. Value output[](https://elv.sh/learn/tour.html#value-output)  

Like in traditional shells, commands in Elvish can output **bytes**. The  
`echo`command outputs bytes:  

```  
~> echofoobarfoo bar  
```  

Additionally, commands can also output **values**. Values include not just  
strings, but also [lambdas](https://elv.sh/learn/tour.html#lambdas),  
[numbers](https://elv.sh/learn/tour.html#numbers),[lists and maps](https://elv.sh/learn/tour.html#lists-and-maps).  
The `put` command outputs values:  

```  
~> putfoo[foo][&foo=bar]{putfoo}▶ foo▶ [foo]▶ [&foo=bar]▶ <closure 0xc000347500>  
```  

Many builtin commands output values. For example, string functions in the  
`str:`module outputs their results as values. This makes those functions work  
seamlessly with strings that contain newlines or even NUL bytes:  

```  
~> usestr~> str:join','["foo\nbar""lorem\x00ipsum"]▶ "foo\nbar,lorem\x00ipsum"  
```  

Unlike most programming languages, Elvish commands don’t have return values.  
Instead, they use the value output to “return” their results.  

Read the reference for [builtin commands](https://elv.sh/ref/builtin.html) to  
learn which commands work with value inputs and outputs. Among them, here are  
some general-purpose primitives:  

| Command                                          | Functionality                                         |  
| ------------------------------------------------ | ----------------------------------------------------- |  
| [`all`](https://elv.sh/ref/builtin.html#all)     | Passes value inputs to value outputs                  |  
| [`each`](https://elv.sh/ref/builtin.html#each)   | Applies a function to all values from value input     |  
| [`put`](https://elv.sh/ref/builtin.html#put)     | Writes arguments as value outputs                     |  
| [`slurp`](https://elv.sh/ref/builtin.html#slurp) | Convert byte input to a single string in value output |  

## 3.2. Value pipelines[](https://elv.sh/learn/tour.html#value-pipelines)  

Pipelines work with value outputs too. When forming pipelines, a command that  
writes value outputs can be followed by a command that takes value inputs. For  
example, the `each` command takes value inputs, and applies a lambda to each one  
of them:  

```  
~> putfoobar|each{|x|echo'I got '$x}I got fooI got bar  
```  

Read the language reference on  
[pipelines](https://elv.sh/ref/language.html#pipeline) to learn more about  
pipelines in general.  

## 3.3. Захват выходного значения[](https://elv.sh/learn/tour.html#value-output-capture)  

Output capture works with value output too. Capturing value outputs always  
recovers the exact values there were written. For example, the `str:join`command  
joins a list of strings with a separator, and its output can be captured and  
saved in a variable:  

```  
~> usestr~> vars=(str:join','["foo\nbar""lorem\x00ipsum"])~> put$s▶ "foo\nbar,lorem\x00ipsum"  
```  

Read the language reference  
on[output capture](https://elv.sh/ref/language.html#output-capture) to learn  
more.  

## 3.4. Lists and maps[](https://elv.sh/learn/tour.html#lists-and-maps)  

Lists look like `[a b c]`, and maps look like `[&key1=value1 &key2=value2]`:  

```  
~> varli=[foobarloremipsum]~> put$li▶ [foo bar lorem ipsum]~> varmap=[&k1=v2&k2=v2]~> put$map▶ [&k1=v2 &k2=v2]  
```  

You can get elements of lists and maps by indexing them. Lists are zero-based  
and support slicing too:  

```  
~> put$li[0]▶ foo~> put$li[1..3]▶ [bar lorem]~> put$map[k1]▶ v2  
```  

Read the language reference on [lists](https://elv.sh/ref/language.html#list)  
and[maps](https://elv.sh/ref/language.html#map) to learn more.  

## 3.5. Numbers[](https://elv.sh/learn/tour.html#numbers)  

Elvish has a number type. There is no dedicated syntax for it; instead, it can  
constructed using the `num` builtin:  

```  
~> num1▶ (num 1)~> num1e2▶ (num 100)  
```  

Most arithmetic commands in Elvish support both typed numbers and strings that  
can be converted to numbers. They usually output typed numbers:  

```  
~> +12▶ (num 3)~> usemath~> math:pow(num10)3▶ (num 1000)  
```  

**Note**: The set of number types will likely expand in future.  

Read the language reference on  
[numbers](https://elv.sh/ref/language.html#number) and the reference for the  
[math module](https://elv.sh/ref/math.html) to learn more.  

## 3.6. Booleans[](https://elv.sh/learn/tour.html#booleans)  

Elvish has two boolean values, `$true` and `$false`.  

Read the language reference on  
[booleans](https://elv.sh/ref/language.html#boolean) to learn more.  

## 3.7. Options[](https://elv.sh/learn/tour.html#options)  

Many Elvish commands take **options**, which look like map pairs (`&key=value`).  
For example, the `echo` command takes a `sep` option that can be used to  
override the default separator of space:  

```  
~> echo&sep=','foobarfoo,bar~> echo&sep="\n"foobarfoobar  
```  

## 3.8. Lambdas[](https://elv.sh/learn/tour.html#lambdas)  

Lambdas are first-class values in Elvish. They can be saved in variables, used  
as commands, passed to commands, and so on.  

Lambdas can be written by enclosing its body with `{` and `}`:  

```  
~> varf={echo"I'm a lambda"}~> $fI'm a lambda~> put$f▶ <closure 0xc000265bc0>~> varg=(put$f)~> $gI'm a lambda  
```  

The opening brace `{`**must** be followed by some whitespace, to disambiguate  
from [brace expansion](https://elv.sh/learn/tour.html#brace-expansion).  

Lambdas can take arguments and options, which can be written in a **signature**:  

```  
~> varf={|ab&opt=default|echo"a = "$aecho"b = "$becho"opt = "$opt}~> $ffoobara = foob = baropt = default~> $ffoobar&opt=optiona = foob = baropt = option  
```  

Read the language reference on  
[functions](https://elv.sh/ref/language.html#function) to learn more about  
functions.  

## 3.9. Control structures[](https://elv.sh/learn/tour.html#control-structures)  

Control structures in Elvish look very different from traditional shells. For  
example, this is how an `if` command looks:  

```  
~> if(eq(uname)Linux){echo"You're on Linux"}You're on Linux  
```  

The `if` command takes a conditional expression (an output capture in this  
case), and the body to execute as a lambda. Since lambdas allow internal  
newlines, you can also write it like this:  

```  
~> if(eq(uname)Linux){echo"You're on Linux"}You're on Linux  
```  

However, you must write the opening brace `{` on the same line as `if`. If you  
write it on a separate line, Elvish would parse it as two separate commands.  

The `for` command looks like this:  

```  
~> forx[expressiveversatile]{echo"Elvish is "$x}Elvish is expressiveElvish is versatile  
```  

Read the language reference on  
[the `if` command](https://elv.sh/ref/language.html#if),[the `for` command](https://elv.sh/ref/language.html#for),  
and additionally[the `while` command](https://elv.sh/ref/language.html#while) to  
learn more.  

## 3.10. Exceptions[](https://elv.sh/learn/tour.html#exceptions)  

Elvish uses exceptions to signal errors. For example, calling a function with  
the wrong number of arguments throws an exception:  

```  
~> varf={echofoo} # doesn't take arguments~> $fabException: arity mismatch: arguments here must be 0 values, but is 2 values[tty 2], line 1: $f a b  
```  

Moreover, non-zero exits from external commands are also turned into exceptions:  

```  
~> falseException: false exited with 1[tty 3], line 1: false  
```  

Exceptions can be caught using the `try` command:  

```  
~> try{false}excepte{echo'got an exception'}got an exception  
```  

Read the language reference  
on[the exception value type](https://elv.sh/ref/language.html#exception)  
and[the `try` command](https://elv.sh/ref/language.html#try) to learn more.  

## 3.11. Namespaces and modules[](https://elv.sh/learn/tour.html#namespaces-and-modules)  

The names of variables and functions can have **namespaces** prepended to their  
names. Namespaces always end with `:`.  

The [using variables](https://elv.sh/learn/tour.html#using-variables) section  
has already shown the `E:`namespace. Other namespaces can be added by importing  
modules with `use`. For example,  
[the `str:` module](https://elv.sh/ref/str.html) provides string utilities:  

```  
~> usestr~> str:to-upperfoo▶ FOO  
```  

You can define your own modules by putting `.elv` files in`~/.config/elvish/lib`  
(or `~\AppData\Roaming\elvish\lib`). For example, to define a module called  
`foo`, put the following in `foo.elv` under the aforementioned directory:  

```  
fnf{echo'in a function in foo'}  
```  

This module can now be used like this:  

```  
~> usefoo~> foo:fin a function in foo  
```  

Read the language reference  
on[namespaces and modules](https://elv.sh/ref/language.html#namespaces-and-modules)  
to learn more.  

## 3.12. External command support[](https://elv.sh/learn/tour.html#external-command-support)  

As shown in examples above, Elvish supports calling external commands directly  
by writing their name. If an external command exits with a non-zero code, it  
throws an exception.  

Unfortunately, many of the advanced language features are only available for  
internal commands and functions. For example:  

- They can only write byte output, not value output.  

- They only take string arguments; non-string arguments are implicitly coerced  
  to strings.  

- They don’t take options.  

Read the language reference  
on[ordinary commands](https://elv.sh/ref/language.html#ordinary-command) to  
learn more about when Elvish decides that a command is an external command.  

Read [the API of the interactive editor](https://elv.sh/ref/edit.html) to learn  
more about UI customization options.  

## 4.1. Tab completion[](https://elv.sh/learn/tour.html#tab-completion)  

Press Tab to start completion. For example, after typing `vim` andSpace, press  
Tab to complete filenames:  

```  
~> cd elvish~/elvish> echo1.0-release.md elf@host COMPLETING argument 1.0-release.md   README.md    syntaxes/CONTRIBUTING.md  SECURITY.md  tools/   Dockerfile       cmd/       vscode/  LICENSE          go.mod       website/ Makefile         go.sumPACKAGING.md     pkg/  
```  

Basic operations should be quite intuitive:  

- To navigate the candidate list, use arrow keys ▲▼◀▶ or Tab and Shift-Tab.  

- To accept the selected candidate, press Enter.  

- To cancel, press Escape.  

As indicated by the horizontal scrollbar, you can scroll to the right to find  
additional results that don’t fit in the terminal.  

You may have noticed that the cursor has moved to the right of “COMPLETING  
argument”. This indicates that you can continue typing to filter candidates. For  
example, after typing `.md`, the UI looks like this:  

```  
~> cd elvish~/elvish> echo1.0-release.md elf@host COMPLETING argument  .md1.0-release.md   PACKAGING.md  SECURITY.mdCONTRIBUTING.md  README.md  
```  

Read the reference on  
[completion API](https://elv.sh/ref/edit.html#completion-api) to learn how to  
program and customize tab completion.  

## 4.2. Command history[](https://elv.sh/learn/tour.html#command-history)  

Elvish has several UI features for working with command history.  

### 4.2.1. History walking[](https://elv.sh/learn/tour.html#history-walking)  

Press ▲ to fetch the last command. This is called **history walking**mode:  

```  
~> math:min 3 1 30elf@host HISTORY #32  
```  

Press ▲ to go further back, ▼ to go forward, orEscape to cancel.  

To restrict to commands that start with a prefix, simply type the prefix before  
pressing ▲. For example, to walk through commands starting with`echo`, type  
`echo` before pressing ▲:  

```  
~> echo(styled warning: red) bumpy roadelf@host HISTORY #2  
```  

### 4.2.2. History listing[](https://elv.sh/learn/tour.html#history-listing)  

Press Ctrl-R to list the full command history:  

```  
~>                                                elf@host HISTORY (dedup on)    3 echo "hello\nbye" > /tmp/x                          │   4 from-lines < /tmp/x                                 │   5 cd /tmp                                                6 cd ~/elvish                                            7 git branch                                             8 git checkout .                                         9 git commit                                            19 git status                                            20 cd /usr/local/bin                                     21 echo $pwd                                             22 * (+ 3 4) (- 100 94)                                  31 make                                                  32 math:min 3 1 30  
```  

Like in completion mode, type to filter the list, press ▲ and▼ to navigate the  
list, Enter to insert the selected entry, or Escape to cancel.  

### 4.2.3. Last command[](https://elv.sh/learn/tour.html#last-command)  

Finally, Elvish has a **last command** mode dedicated to inserting parts of the  
last command. Press Alt-, to trigger it:  

```  
~> echo abc defabc def~> vimelf@host LASTCMD     echo abc def                                            0 echo  1 abc  2 def  
```  

## 4.3. Directory history[](https://elv.sh/learn/tour.html#directory-history)  

Elvish remembers which directories you have visited. Press Ctrl-L to list  
visited directories. Use ▲ and ▼ to navigate the list, Enter to change to that  
directory, or Escape to cancel.  

```  
~>                                                elf@host LOCATION  10 ~/elvish                                              10 ~/.local/share/elvish                                 10 ~/elvish/website                                      10 ~/.config/elvish                                       9 ~/elvish/pkg/edit                                      9 ~/elvish/pkg/eval                                      9 /opt                                                   9 /usr/local                                             9 /usr/local/share                                       9 /usr/local/bin                                         9 /usr                                                   9 /tmp                                                 │  8 ~/zsh                                                │  
```  

Type to filter:  

```  
~>                                                elf@host LOCATION  local 10 ~/.local/share/elvish                                   9 /usr/local  9 /usr/local/share  9 /usr/local/bin  
```  

## 4.4. Navigation mode[](https://elv.sh/learn/tour.html#navigation-mode)  

Press Ctrl-N to start the builtin filesystem navigator.  

```  
~/elvish>                                         elf@host NAVIGATING  bash    1.0-release.md   1.0 has not been released yet. elvish   CONTRIBUTING.md  zsh      Dockerfile                LICENSE                   Makefile                  PACKAGING.md              README.md                 SECURITY.md      cmd                       go.mod                    go.sum           pkg             │ syntaxes        │  
```  

Unlike other modes, the cursor stays in the main buffer in navigation mode. This  
allows you to continue typing commands; while doing that, you can pressEnter to  
insert the selected filename. You can also pressAlt-Enter to insert the filename  
without exiting navigation mode; this is useful when you want to insert multiple  
filenames.  

## 4.5. Startup script[](https://elv.sh/learn/tour.html#startup-script)  

Elvish’s interactive startup script is  
[`rc.elv`](https://elv.sh/ref/command.html#rc-file). Non-interactive Elvish  
sessions do not have a startup script.  

### 4.5.1. POSIX aliases[](https://elv.sh/learn/tour.html#posix-aliases)  

Elvish doesn’t support POSIX aliases, but you can get a similar experience  
simply by defining functions:  

```  
fnls{|@a|e:ls--color$@a}  
```  

The `e:` prefix (for “external”) ensures that the external command named  
`ls`will be called. Otherwise this definition will result in infinite recursion.  

### 4.5.2. Prompt customization[](https://elv.sh/learn/tour.html#prompt-customization)  

The left and right prompts can be customized by assigning functions  
to[`edit:prompt`](https://elv.sh/ref/edit.html#$edit:prompt)  
and[`edit:rprompt`](https://elv.sh/ref/edit.html#$edit:rprompt). The following  
example defines prompts similar to the default, but uses fancy Unicode.  

```  
~> setedit:rprompt=(constantly ^(styled(whoami)✸(hostname) inverse))~> setedit:prompt={tilde-abbr$pwdstyled'❱ ' bright-red}~❱ # Fancy unicode prompts!elf✸host.example.com  
```  

The [`tilde-abbr`](https://elv.sh/ref/builtin.html#tilde-abbr) command  
abbreviates home directory to a tilde. The  
[`constantly`](https://elv.sh/ref/builtin.html#constantly) command returns a  
function that always writes the same value(s) to the value output.  
The[`styled`](https://elv.sh/ref/builtin.html#styled) command writes styled  
output.  

### 4.5.3. Changing PATH[](https://elv.sh/learn/tour.html#changing-path)  

Another common task in the interactive startup script is to set the search path.  
You can do set the environment variable directly (all environment variables have  
a `E:` prefix):  

```  
setE:PATH=/opts/bin:/bin:/usr/bin  
```  

But it is usually nicer to set the  
[`$paths`](https://elv.sh/ref/builtin.html#$paths)instead:  

```  
setpaths=[/opts/bin/bin/usr/bin]  
```  

Elvish has its own set of [builtin commands](https://elv.sh/ref/builtin.html).  
This section helps you find commands that correspond to commands in traditional  
shells.  

## 5.1. command[](https://elv.sh/learn/tour.html#command)  

To force Elvish to treat a command as an external command, prefix it  
with[`e:`](https://elv.sh/ref/language.html#special-namespaces).  

## 5.2. export[](https://elv.sh/learn/tour.html#export)  

In Elvish, environment variables live in  
the[`E:`](https://elv.sh/ref/language.html#special-namespaces) namespace. There  
is no concept of exporting a variable to the environment; environment variables  
are always “exported” to child processes, and non-environment variables never  
are.  

## 5.3. source[](https://elv.sh/learn/tour.html#source)  

To build reusable libraries, use  
Elvish’s[module mechanism](https://elv.sh/ref/language.html#modules).  

To execute a dynamic piece of code for side effect,  
use[`eval`](https://elv.sh/ref/builtin.html#eval). If the code lives in a file,  
write`eval (slurp < /path/to/file)`.  

Due to Elvish’s scoping rules, files executed using either of the mechanism  
above can’t create new variables in the current namespace. For  
example,`eval 'var foo = bar'; echo $foo` won’t work. However, the REPL’s  
namespace*can* be manipulated with  
[`edit:add-var`](https://elv.sh/ref/edit.html#edit:add-var).  

## 5.4. test[](https://elv.sh/learn/tour.html#test)  

To test files, use commands in the [path](https://elv.sh/ref/path.html) module.  

To compare numbers,  
use[number comparison commands](https://elv.sh/ref/builtin.html#num-cmp).  

To compare strings,  
use[string comparison commands](https://elv.sh/ref/builtin.html#str-cmp).  

To perform boolean operations,  
use[`and`](https://elv.sh/ref/language.html#and-or-coalesce),[`or`](https://elv.sh/ref/language.html#and-or-coalesce)  
or[`not`](https://elv.sh/ref/builtin.html#not). **Note**: `and` and `or` are  
part of the language rather than the builtin module, since they  
perform[short-circuit evaluation](https://en.wikipedia.org/wiki/Short-circuit_evaluation)and  
don’t always evaluate all the arguments.  

## 5.5. which[](https://elv.sh/learn/tour.html#which)  

To check if an external command exists,  
use[has-external](https://elv.sh/ref/builtin.html#has-external).  

To query the path of an external command,  
use[search-external](https://elv.sh/ref/builtin.html#search-external).  
