# Paper Template

meireles@umn.edu

--

General template for my academic papers. Its main features are:

1. Uses git for version control;
2. Taps into my centralized bibliography repo [`bibtexlib`](https://github.com/meireles/bibtexlib).
3. Is compatible with the online collaborative TeX platform [Overleaf](https://www.overleaf.com/).
4. Can be embedded into git controlled R projects; see [research\_project\_template](https://github.com/meireles/research_project_template) and  [scifi](https://github.com/meireles/scifi).


#### Catches
The workflow relies heavily on third party git tools:

1. [`bibtexlib`](https://github.com/meireles/bibtexlib) is embedded using non-standard git commands in [`git subrepo`](https://github.com/ingydotnet/git-subrepo).

2. Embedding this template in an R project template relies on [scifi](https://github.com/meireles/scifi) and on [git-utils](https://github.com/meireles/git-utils).

## Structure

* __main.tex:__ Manuscript itself.

* __figures:__ Should be **the only** directory holding the images that go into the manuscript.

* __tables:__ Houses **all** `.csv` files to be rendered as tables in the manuscript.

* __references__ Should be the **only** directory sourced for bibtex `.bib` libraries. It is divided into two sub-directories:

  * [__bibtexlib:__](https://github.com/meireles/bibtexlib) A [`git subrepo`](https://github.com/ingydotnet/git-subrepo) of my centralized bibliography repository. This repo __evolves slowly__, and __shouldn't be written to__ from a `paper_template` instance (at least not very often).

* __.misctmp__ hidden directory used for temporary files. For example, the word counting macros write files to it. 

## Workflows

This is a template and thus is meant to be instantiated. There are two ways I do this: 1) instantiating it inside a research project like [scifi](https://github.com/meireles/scifi) (99% of the time), or 2) instantiating  it directly, by 'forking' and cloning.

### Workflow 1 (preferred)

Really nothing to do here. I simply instantiate my [research\_project\_template](https://github.com/meireles/research_project_template) using [scifi](https://github.com/meireles/scifi), and I have an R project set up with my manuscript inside. **If** I choose to use overleaf with it, the [scifi](https://github.com/meireles/scifi) instantiated project will have a `init-overleaf` script that helps to set things up.

### Workflow 2 (rarely used)

### 1. Basic set up

1. __Fork this remote__, giving it a name that makes sense for the paper, say `my_paper`
2. __Clone the `my_paper` fork to a local repo__.
3. __Sanity check:__ Do a `git remote -v` and a `git subrepo status --all` at the root of `my_paper`.

```
Meireles:my_paper Meireles$ git remote -v
origin	https://meireles@bitbucket.org/meireles/my_paper.git (fetch)
origin	https://meireles@bitbucket.org/meireles/my_paper.git (push)

Meireles:my_paper Meireles$ git subrepo status --all
1 subrepo:
Git subrepo 'references/bibtexlib':
  Remote URL:      https://github.com/meireles/bibtexlib
  Tracking Branch: master
  Pulled Commit:   c6b2712
  Pull Parent:     6e27c16
```

### 2. Add repo to Overleaf (optional)

1. Log in to [Overleaf](https://www.overleaf.com/) and create a new **blank** project. I'll call it `my_paper`.
2. By default Overleaf adds a `main.tex` with a basic outlines to you project. __Make sure to delete the contents of `main.tex`__, otherwise you'll have to resolve a conflict when trying to `git pull`.
3. Get the git link <overleaf_git_link> from the `Share` menu.
4. Set up Overleaf (see code below)
    1. Add Overleaf as a remote for `my_paper`. Name the remote `overleaf`!
    2. Pull contents from Overleaf's project (usually their default `main.tex`).
    3. Revert to previous commit.
    4. Push `my_paper` to the Overleaf remote `overleaf`!

```
git remote add overleaf <overleaf_git_link>  # name the remote overleaf!
git checkout master                          # Make sure to be in master
git pull overleaf master                     # May complain about conflicts if I forget to delete 
                                             # the contents of Overleaf's default main.tex.
git revert --mainline 1 HEAD                 # reverts the commit generated by `git pull`
git push overleaf master                     # Now just push it!
```

### 3. Work normally...

1. The cycle of `pull`, write, `commit`, `push` happens normally, __without worrying about the `subrepo`.__
2. If Overleaf is set up, remember that:
    1. I'll have two remotes to `push` and `pull` from!
    2. Overleaf creates `commits` from their web interface when you snapshot a version -- so you need to remember to `pull` changes!

### 4. Updating your local `bibtexlib`.

To pull changes made to the remote `bibtexlib`, run the following (at `my_paper` root):

```
git subrepo pull references/bibtexlib/
```

**Caution:** Must watch out for updates in bibtex tags. A change from `meireles2016` to `meireles2016a` can mess up my manuscript.

### 5. Pushing local changes to `bibtexlib`!

In principle, **I will avoid** making changes to `bibtexlib` from a `paper_template` fork.

If these changes in `subrepos` happen though, I can push them to [bibtexlib](https://github.com/meireles/bibtexlib) with:

```
git subrepo push references/bibtexlib/
```

---

## Sausage factory: Adding `bibtexlib` as a subrepo 

This just documents how I added [`bibtexlib`](https://github.com/meireles/bibtexlib) as a subrepo to this project.

1. Installed [git-subrepo](https://github.com/ingydotnet/git-subrepo).
2. `git subrepo clone https://github.com/meireles/bibtexlib references/bibtexlib`

And that's it! Much easier than _submodules_ or _subtrees_.
