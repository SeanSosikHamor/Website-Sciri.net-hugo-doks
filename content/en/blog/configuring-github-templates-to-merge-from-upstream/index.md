---
title: "Configuring Github Templates to Merge From Upstream"
description: "Unlike forking a repository, which includes the entire commit history of the parent repository, clicking Use this template on GitHub starts a new repository with a single commit and an unrelated git history."
lead: "Unlike forking a repository, which includes the entire commit history of the parent repository, clicking Use this template on GitHub starts a new repository with a single commit and an unrelated git history."
date: 2022-04-08T12:13:48-04:00
draft: false
weight: 50
images: ["configuring-github-templates-to-merge-from-upstream.jpg"]
contributors: []
---

## GitHub Templates Diverge From Upstream Repositories

[Creating a repository from a template](https://docs.github.com/en/enterprise-server/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template) starts a new repository with a single commit and no history. This is perfect for starting brand-new, unrelated projects, but makes it difficult to merge changes and updates from upstream.

For example, [generating a new repository from a Jamstack GitHub template](https://github.com/h-enk/doks/generate) can get a site up and running quickly, but it may be difficult to merge future Jamstack theme updates and changes. This can be solved by associating the remote, upstream repository before making any changes.

```
$ git remote -v
origin    https://github.com/SeanSosikHamor/Website-Sciri.net-hugo-doks.git (fetch)
origin    https://github.com/SeanSosikHamor/Website-Sciri.net-hugo-doks.git (push)
upstream    https://github.com/h-enk/doks.git (fetch)
upstream    https://github.com/h-enk/doks.git (push)
``` 

## Starting Fresh With a Brand-New GitHub Template

Starting fresh is the path of least resistance, and doesn't require any time-consuming merge operations if the remote upstream is specified before local changes are made.

Using the Jamstack [doks](https://github.com/h-enk/doks) theme as an example, [create a new repository from the template](https://github.com/h-enk/doks/generate), clone the newly-created repository, then specify the remote upstream:

```
$ git clone https://github.com/SeanSosikHamor/doks-remote-upstream-example.git
$ cd doks-remote-upstream-example
$ git remote add upstream https://github.com/h-enk/doks
```

Check to make sure that the new upstream remote has been added:

```
$ git remote -v
> origin    https://github.com/SeanSosikHamor/doks-remote-upstream-example.git (fetch)
> origin    https://github.com/SeanSosikHamor/doks-remote-upstream-example.git (push)
> upstream    https://github.com/h-enk/doks (fetch)
> upstream    https://github.com/h-enk/doks (push)
```

Treat the newly-created repository as a fork, and start the sync with upstream:

```
$ git fetch upstream
$ git checkout master
```

This is where things get tricky, because a template is not a fork, and has an unrelated git history:

```
$ git merge upstream/master
> fatal: refusing to merge unrelated histories
```

The error can be resolved by toggling the allow-unrelated-histories switch:

```
$ git pull origin master --allow-unrelated-histories
>  * branch            master     -> FETCH_HEAD
> Already up to date.

$ git pull upstream master --allow-unrelated-histories
> From https://github.com/h-enk/doks
>  * branch            master     -> FETCH_HEAD
> Merge made by the 'recursive' strategy.
```

If there were no local changes, then the merge will immediately commit and prompt for a commit message. The merge can now be pushed back to the newly-created repository:

```
$ git status
> On branch master
> Your branch is ahead of 'origin/master' by 766 commits.
>   (use "git push" to publish your local commits)
> 
> nothing to commit, working tree clean

$ git push
> Enumerating objects: 3530, done.
> Counting objects: 100% (3528/3528), done.
> Delta compression using up to 16 threads
> Compressing objects: 100% (1418/1418), done.
> Writing objects: 100% (3379/3379), 2.68 MiB | 8.05 MiB/s, done.
> Total 3379 (delta 2004), reused 3266 (delta 1901), pack-reused 0
> remote: Resolving deltas: 100% (2004/2004), completed with 104 local objects.
> To https://github.com/SeanSosikHamor/doks-remote-upstream-example.git
>    77a68ec..399014f  master -> master
```

## Merging With Upstream After Changes Have Been Made

If a project has already been customized and deployed, it may be tedious after the fact to manually merge in all the changes where the customized repository has diverged from the upstream repository. Every local file that's been modified will be treated as a change, even if the upstream file hasn't been changed since the initial template was cloned.

```
$ git remote add upstream https://github.com/h-enk/doks
$ git remote -v                                        
> origin    https://github.com/SeanSosikHamor/Website-Sciri.net-hugo-doks.git (fetch)
> origin    https://github.com/SeanSosikHamor/Website-Sciri.net-hugo-doks.git (push)
> upstream    https://github.com/h-enk/doks (fetch)
> upstream    https://github.com/h-enk/doks (push)

$ git fetch upstream
$ git checkout master
$ git merge upstream/master
> fatal: refusing to merge unrelated histories

$ git pull upstream master --allow-unrelated-histories
> From https://github.com/h-enk/doks
>  * branch            master     -> FETCH_HEAD
> CONFLICT (add/add): Merge conflict in package-lock.json
> Auto-merging package-lock.json
> CONFLICT (add/add): Merge conflict in netlify.toml
> Auto-merging netlify.toml
> CONFLICT (add/add): Merge conflict in layouts/sitemap.xml
> Auto-merging layouts/sitemap.xml
```

## Resolving Git Merge Conflicts

There are many workflows and personal preferences when it comes to resolving merge conflicts, and [GitHub has a basic tutorial](https://docs.github.com/en/enterprise-server/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line).

## Other Methods

The above steps essentially convert a template into a fork, which may not be desired. Other methods have been outlines by other developers:

- [GitHub - Pull changes from a template repository](https://stackoverflow.com/questions/56577184/github-pull-changes-from-a-template-repository)
- [How to use Git to downstream changes from a template](https://medium.com/geekculture/how-to-use-git-to-downstream-changes-from-a-template-9f0de9347cc2) 

## Related Documentation
- [Creating a repository from a template](https://docs.github.com/en/enterprise-server/repositories/creating-and-managing-repositories/creating-a-repository-from-a-template)
- [Configuring a remote for a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/configuring-a-remote-for-a-fork)
- [Syncing a fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/syncing-a-fork)
- [The "fatal: refusing to merge unrelated histories" Git error](https://www.educative.io/edpresso/the-fatal-refusing-to-merge-unrelated-histories-git-error)
- [Resolving a merge conflict using the command line](https://docs.github.com/en/enterprise-server/pull-requests/collaborating-with-pull-requests/addressing-merge-conflicts/resolving-a-merge-conflict-using-the-command-line)
