---
layout: post
title: Gitweb links in DokuWiki
tags: gitweb, dokuwiki, gwlinks
commentIssueId: 1
---

Azzurra has recently transitioned from [Mercurial](http://mercurial.selenic.com/) to [Git](http://git-scm.com/) for its public repositories.
Among other things, this meant that every link to a commit in the [development wiki](http://devel.azzurra.org/) was outdated and ought to be replaced.

When I originally set up the wiki, I chose its so called _interwiki links_ to create links to commits.

For instance, `[[baha>partial-hash]]` would be expanded (more or less) to

{% highlight html %}
<a href="http://devel.azzurra.org/hg/bahamut/rev/partial-hash">
  <img src="url/to/interwiki/baha.png" /> partial-hash
</a>
{% endhighlight %}

This meant that for each project I had to create an entry into `conf/interwiki.local.conf` and put the right badge into `lib/images/interwiki/`. Ew.

After switching over to Git, I found that gitweb URLs are quite more complicated than their mercurial counterparts - and I wanted a generic solution to this problem, not
hacking the wiki's configuration for each and every project added to the repository.

Fortunately enough, DokuWiki's syntax can be extended [quite easily](http://www.dokuwiki.org/devel:syntax_plugins), so I thought that I could write something that could
allow me to write

     [[Git?bahamut.git#partial-hash|This]] is the commit you're looking for

and have it expanded into

{% highlight html %}
<a href="http://devel.azzurra.org/git/?p=bahamut.git;a=commit;h=partial-hash">
 This
</a>
is the commit you're looking for
{% endhighlight %}

In the end, it all boiled down to a (nasty) regexp and some additional PHP
code.

You can see it in all its glory
[here](https://github.com/rfc1459/gwlinks).
