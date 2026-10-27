hhieu.xyz

- `npx quartz build --serve` : localhost:8080
- Mainly edit content in Obsidian, edit layout in `quartz.layout.ts`, edit more config in `quartz.config.ts`

### git commands to update content

```
git add content*; git status
git commit -m "Content update"; git push
```

### sync changes to git

```
git pull
npx quartz sync
```

### merge changes from upstream

```
git fetch upstream
git checkout v4
git merge upstream/v4

# after you're done making all the changes:
git push

## if having issues with packages etc
npm ci
```

h/t [git - How can I merge changes from an upstream branch to my fork’s branch - Stack Overflow](https://stackoverflow.com/questions/52981111/how-can-i-merge-changes-from-an-upstream-branch-to-my-forks-branch)

A good practice is to have this structure, which looks like you do, but is good to explain for clarification purposes:

```
$ git remote -v

origin  https://github.com/your-username/forked-repository.git (fetch)
origin  https://github.com/your-username/forked-repository.git (push)
upstream    https://github.com/original-owner-username/original-repository.git (fetch)
upstream    https://github.com/original-owner-username/original-repository.git (push)
```

So `origin` is your fork and `upstream` the original repository. Then use

```
git fetch upstream
```

And you will get this output

```
From https://github.com/original-owner-username/original-repository
 * [new branch]      main     -> upstream/main
```

where you have now a branch called `upstream/main` which tracks the original repo.

Then checkout to your local fork branch and merge

```
git checkout main
git merge upstream/main
```


