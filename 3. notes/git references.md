source: https://git-scm.com/docs
### git downstream / upstream

tl;dr

- information flows "downstream" to you (clone, pull)
- make changes by sending them "upstream" (add, commit, push)

source: https://stackoverflow.com/questions/2739376/definition-of-downstream-and-upstream

> In terms of source control, you're downstream when you copy (clone, checkout, etc) from a repository. Information flowed "downstream" to you.
> 
> When you make changes, you usually want to send them back "upstream" so they make it into that repository so that everyone pulling from the same source is working with all the same changes. This is mostly a social issue of how everyone can coordinate their work rather than a technical requirement of source control. You want to get your changes into the main project so you're not tracking divergent lines of development.
> 
> Sometimes you'll read about package or release managers (the people, not the tool) talking about submitting changes to "upstream". That usually means they had to adjust the original sources so they could create a package for their system. They don't want to keep making those changes, so if they send them "upstream" to the original source, they shouldn't have to deal with the same issue in the next release.


---

