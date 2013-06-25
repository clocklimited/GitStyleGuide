# Git branch naming convention

Furthr to the Git Flow branching model, I propose the following style, workflow
and naming convention for Git branches.

## Constant Branches

Repos will always have these branches.

master - Stable production version

develop - Main branch for all development

## Release Branches

release/{release-name}

### Examples

release/sprint2
release/launch
release/phase2
release/user-profile-upgrade

These should be branched from develop once an agreed amount of completeness is
reached. develop is not again merged into the feature branch, but small changes
may be merged from other feature branches if something is missed or a long time
elapses between creation and deployment. Once all checks pass this
should be merge onto develop and master. The release name should be defined by
the product owner and never be the version number.

## Feature Branches

feature/{feature-name}

### Examples

feature/profile-page
feature/access-control

The feature you are working on. Normally branched from develop during sprints,
or from a release or possibly a hotfix if something is missed. It is very
important that you *only merge from the branches that you originally branched
from and do not introduce other unrelated features when your feature is merged
in.

## Hotfix Branches

hotfix/{reason}

### Examples
hotfix/broken-images
hotfix/ad-rename

Hot fixes are changes that resolve issues and add essential features
outside of the planned release schedule.
They should be branched from master and merged back to master, develop
and any pending release branches to prevent regression.

## Versioning

Before any deployment you should tag your release with a version number.

    git tag 48.rc1
    git push --tags

This should increment a full version with every release. **1, 2, 3 etc** This is
not to be confused with the release names, **phase 2** may well be version **48**.

You may need to show a stakeholder a version on testing this should be either an
[alpha](http://en.wikipedia.org/wiki/Software_release_life_cycle#Alpha) or
[beta](http://en.wikipedia.org/wiki/Software_release_life_cycle#Beta) version
with the tag: **48.alpha1**, **48.beta1**, **48.beta2**. Alpha can be any partial feature
that you want to simply show some progress of. beta releases should pass some
basic quality assurance and have the potential to become a releace candidate.

The first version of release to go on staging should be the first
[release candidate](http://en.wikipedia.org/wiki/Software_release_life_cycle#Release_candidate):
**48.rc1** this should then increase, **48.rc2**, **48.rc3**, **48.rc4** until
the release is deployed and signed off. At which point the last release
candidate can should be tags as the final release.

    git checkout 48.rc4
    git tag 48
    git push --tags

Then merged back on to develop and master:

    git checkout develop
    git pull origin develop
    git merge 48
    git push origin develop
    git checkout master
    git pull origin master
    git merge 48
    git push origin master

Hotfixes should increase the [minor version number](http://en.wikipedia.org/wiki/Software_versioning)

  git checkout hotfix/ad-rename
  git tag 48.1.rc1
  git push --tags