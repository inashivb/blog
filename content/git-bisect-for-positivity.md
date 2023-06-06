---
title: "Git Bisect for Positivity"
date: 2023-06-06T19:39:57+05:30
draft: true
---

Hello hello hello!

Today, we're going to be talking about Git Bisect.
You'd wonder, what's so special about it? Everybody uses it every now and then,
yes, but for what?

# What do we use git bisect for?
For finding the evil commit that introduced a bug!

<insert evil smirk gif>

Buuuuuuuuuuuut, the post's title suggests that this is about using git bisect for
"positivity". So, instead of the usual usecase (which is also very important btw),
we're going to see how to use git bisect for finding the commit that introduced the
fix.

<insert flying ecstatic gif>

# Story

Well, I thought that git bisect could only be used to find out the commit that
introduced the “bug”. But, bein the naive praani that I am, I tried to use the commands
that I knew to my usecase only to find out they don’t work.

The error I got in that situation was:
```
Some good revs are not ancestors of the bad rev.
git bisect cannot work properly in this case.
Maybe you mistook good and bad revs?
```

Which made sense. I had a bad commit say `0blahbad` say 700 commits ago and somewhere
in those 700 commits, it became good. And, according to git bisect, you are trying to
find out which commit introduced the bug so, the bad commit must be more recent than
the good one and not the other way around.
Makes sense?

<Insert meme super confused>

So, I thought, ok, let’s swap out good for bad and bad for good and that’ll probably
do the job, right? But, well, in the middle of doing that, I lost the context because
I have the attention span of a goldfish and I could no longer count on myself whether
I was marking good commits as good or bad commits as good (as I should have).

<insert meme shoot self>

So… I moved to Stack Overflow for assistance. From the response that I received there,
it seemed like not many folks have tried to use git bisect to find out the commit
that introduced a fix.
https://stackoverflow.com/questions/75407638/how-to-git-bisect-where-bad-commit-is-ancestor-of-good-commit-find-the-fix-com/75408085 and I got the approach I had tried earlier as an answer: https://stackoverflow.com/a/75407777/3715736

<insert meme getting validation from someone with 30k+ rep>

Then, I learned that you can define the “terms”  in git bisect. So, if bad = good and
good = bad was confusing, you could rename it to something more suited to your
understanding that helps you retain the context.

```
git bisect start --term-good mybad --term-bad mygood

and then, later on,
git bisect mybad 0c5c211ba # for our bad but really a good commit
git bisect mygood d04eb09ab # for our good but really a bad commit

```

And, it seemed like the answer to all my problems. Right then, I spotted something in the
man page:
```
Sometimes you are not looking for the commit that introduced a breakage, but rather
for a commit that caused a change between some other "old" state and "new" state.
```

So… turns out git bisect does already let you find out the commit that introduced a fix.
It’s just not as popular as finding the bugs (this shows how we focus on the bugs,
we all need to calm down :P).

You need to do,

```
git bisect start
git bisect old <commit-with-old-behavior>
git bisect new <commit-with-new-behavior>
```

And, then, you continue like you would in your regular bisect, just instead of
good or bad, you’d say old or new depending on whether the behavior you see on
the current commit is old or new as per your expectations.

And, that’s it for today’s What The S! Moments. 😓
