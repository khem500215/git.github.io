
## Welcome to the first edition of Git Rev News!

[Git Rev News](http://git.github.io/rev_news/rev_news.html) is written
collaboratively on [GitHub](https://github.com/git/git.github.io) by
volunteers. Feel free to help by sending [pull
requests](https://github.com/git/git.github.io/pulls) or opening
[issues](https://github.com/git/git.github.io/issues)!

## Reviews & Support

### Reviews

* [make git-pull a builtin](http://thread.gmane.org/gmane.comp.version-control.git/265628/)

Paul Tan, who will probably apply to be a [Google Summer of Code
(GSoC)](https://developers.google.com/open-source/soc/) student for
Git this year, anticipated the start of the GSoC and sent a patch to
rewrite git-pull.sh as a built-in.

Indeed as stated in [the GSoC idea
page](http://git.github.io/SoC-2015-Ideas.html):

```
Many components of Git are still in the form of shell and Perl
scripts. While this is an excellent choice as long as the
functionality is improved, it causes problems in production code – in
particular on multiple platforms, e.g. Windows (think:
POSIX-to-Windows path conversion issues).
```

This is why the "Convert scripts to builtins" GSoC project idea is to:

```
... dive into the Git source code and convert a couple of shell
and/or Perl scripts into portable and performant C code, making it a
so-called "built-in".
```

It appeared that two developers, Duy Nguyen and Stephen Robin, had
already worked on converting git-pull.sh into a built-in in the
past. This happens quite often, so it is a good idea before starting
to develop something in Git, to search and ask around.

Johannes "Dscho" Schindelin, one of the Git for Windows developers, 
thanked Paul for the very detailed and well researched comments in 
his patch and commented a bit on the benefits of this kind of work:

```
> on Windows the runtime fell from 8m 25s to 1m 3s.

This is *exactly* the type of benefit that makes this project so important! Nice one.

> A simpler, but less perfect strategy might be to just convert the shell
> scripts directly statement by statement to C, using the run_command*()
> functions as Duy Nguyen[2] suggested, before changing the code to use
> the internal API.

Yeah, the idea is to have a straight-forward strategy to convert the scripts in as efficient manner as
possible. It also makes reviewing easier if the first step is an almost one-to-one translation to
`run_command*()`-based builtins.

Plus, it is rewarding to have concise steps that can be completed in a timely manner.
```

Regarding the test suite, Matthieu Moy suggested:

```
> Ideally, I think the solution is to
> improve the test suite and make it as comprehensive as possible, but
> writing a comprehensive test suite may be too time consuming.

time-consuming, but also very beneficial since the code would end up
being better tested. For sure, a rewrite is a good way to break stuff,
but anything untested can also be broken by mistake rather easily at any
time.

I'd suggest doing a bit of manual mutation testing: take your C code,
comment-out a few lines of code, see if the tests still pass, and if
they do, add a failing test that passes again once you uncomment the
code.
```

Junio also reviewed some parts of the patch.

* [Forbid "log --graph --no-walk"](http://thread.gmane.org/gmane.comp.version-control.git/264899/)

Dongcan Jiang, who will also probably apply to be a GSoC student for
Git this year, sent a patch to prevent `git log` from being used with both
the `--graph` and the `--no-walk` option. He sent this patch because
the Git community asks potential students to work on a
[microproject](http://git.github.io/SoC-2015-Microprojects.html) to
make sure that they can work properly with the community.

Eric Sunshine made some good general suggestions that are often made
on incoming patches:

```
> Forbid "log --graph --no-walk

Style: drop capitalization in the Subject: line. Also prefix with the
command or module being modified, followed by a colon. So:

    log: forbid combining --graph and --no-walk

or:

    revision: forbid combining --graph and --no-walk

> Because --graph is about connected history while --no-walk is about discrete points.

Okay. You might also want to cite the wider discussion[1].

[1]: http://article.gmane.org/gmane.comp.version-control.git/216083

> revision.c: Judge whether --graph and --no-walk come together when running git-log.
> buildin/log.c: Set git-log cmd flag.
> Documentation/rev-list-options.txt: Add specification on the forbidden usage.

No need to repeat in prose what the patch itself states more clearly
and concisely.

Also, such a change should be accompanied by new test(s).
```

René Scharfe and Junio also suggested some improvements especially in the code.

Junio also explained the interesting behavior of `git show` depending
on the arguments it is given:

```
When "git show" is given a range, it turns no-walk off and becomes a
command about a connected history.  Otherwise, it is a command about
discrete point(s).
```

### Support

* [Git with Lader logic](http://thread.gmane.org/gmane.comp.version-control.git/265679/)
 
People often ask the Git mailing list whether they can use Git to manage
special content. Fortunately, the list is monitored by many experts in different
domains who can often provide specific answers.

This time Bharat Suvarna asked about [PLC
programs](http://en.wikipedia.org/wiki/Programmable_logic_controller)

Kevin D gave the usual non-specific answer:

```
Although git is not very picky about the contents, it is optimized to
track text files. Things like showing diffs and merging files only works
on text files.

Git can track binary files, but there are some disadvantages:

- Diff / merge doesn't work well
- Compression is often difficult, so the repository size may grow
  depending on the size of the things stored
```

Then Randall S. Becker gave a more specific answer:

```
Many PLC programs either store their project code in XML, L5K or L5X (for
example), TXT, CSV, or some other text format or can import and export to
text forms. If you have a directory structure that represents your project,
and the file formats have reasonable line separators so that diffs can be
done easily, git very likely would work out for you. You do not have to have
the local .git repository in the same directory as your working area if your
tool has issues with that or .gitignore. You may want to use a GUI client to
manage your local repository and handle the commit/push/pull/merge/rebase
functions as I expect whatever PLC system you are using does not have git
built-in.
```

Doug Kelly also gave some interesting specific information and pointed to a
web site more information.

Thank you all for these helpful answers!

## Misc Git News

### Developers

* David Kastrup (dak at gnu.org) previously reimplemented significant
parts of "git blame" for a vast gain in performance with complex
histories and large files. As working on free software is his sole
source of income, please consider contributing to his remuneration if
you find this kind of improvements useful.

### Events

* [Git Merge 2015](http://git-merge.com/), The Conference for the Git
Community, will take place on April 8th & 9th in Paris, France, thanks
to GitHub and the sponsors. If you are a Git developer and need travel 
or other assistance to go there, please contact Peff.

### Companies

