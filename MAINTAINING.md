# Maintaining the Bigby.office fork

Bigby.office is Collabora Online with our branding and fixes on top. This note covers how we keep it up to date with Collabora's releases.

## Branches and tags

- `main` is what we deploy: the latest Collabora 26.04 release tag plus our own commits.
- Collabora's code comes from their stable release branch, `distro/collabora/co-26.04`, using the release tags (`cp-26.04.x-y`). We don't track their development branch.
- Every deploy gets a tag like `bigby-26.04.5-2-1` (Collabora release, then our build number), so the code running on bigby.cloud can always be found here.

## One-off setup per clone

```
git remote add upstream https://github.com/CollaboraOnline/online.mirror.git
git config rerere.enabled true
```

Collabora develop on their own Gerrit server. `online.mirror` is their read-only copy on GitHub. The `CollaboraOnline/online` repo on GitHub only holds Helm charts now.

`rerere` makes git remember how we resolved a conflict, so the same conflict is fixed automatically next time.

## Taking a new Collabora release

Check for new tags every two weeks, and straight away when there's a security fix.

```
git fetch upstream --tags
git tag -l 'cp-26.04.*' --sort=v:refname | tail -3

git checkout -b sync/cp-26.04.6-1 main
git merge cp-26.04.6-1
```

Fix any conflicts, build, and test. Then open a pull request into `main`, merge it, tag it and deploy.

Always merge, never rebase `main`. The repo is public, so rewriting history breaks other people's clones. The merge commit also records which Collabora release we took in.

## Moving to the next major version

Once a year Collabora start a new release branch (27.04 and so on). Start a fresh branch from its first tag and cherry-pick our commits across. This lists them:

```
git log --no-merges --oneline <last cp tag we merged>..main
```

## Keeping our changes easy to carry

- Prefer configuration and the `branding/` folder to editing Collabora's files.
- When we do edit their files, keep each commit small and about one thing.
- Don't reformat or tidy code we aren't otherwise changing. It causes conflicts on every update.
- If a fix would help everyone, think about sending it to Collabora through Gerrit, so we don't have to carry it.

## Building with our branding

```
./configure --with-app-branding=$PWD/branding
```
