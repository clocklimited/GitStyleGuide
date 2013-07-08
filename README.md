# Clock Branching and Release Methodology

## tl;dr

Clock follow [gitflow](http://nvie.com/posts/a-successful-git-branching-model/) but
use '/' instead of '-' to group branch types and we encourage developers to push
their branches to origin at least daily.

## Long Version

Clock's branching and release methodology very closely follows the
[gitflow](http://nvie.com/posts/a-successful-git-branching-model/) method. Our
development falls into three broad project types: *initial builds*, *phased
updates* and *maintenance work*, and it is important that our method works for
all types. On a single project several concurrent features can be under
development by one or more teams. Some clients are happy to agree a sprint and
accept the delivered features as a release at the end of the sprint. Other
clients change their priorities on a daily basis so features get dropped and
added from a release at short notice. We are committed to the highest level of
client satisfaction and ensuring a premium customer experience, so it's vital
that our processes have the flexibility to ensure we can deliver on that
commitment.

On top of gitflow we have our own in-house definitions and a slightly modified
naming convention.

## Main Branches

Repos will **always** have these branches:

### master

This is what is in production. Before a site goes live for the first time this
will be the most up to date staging release.

### develop

Receives all features during an active development phase of a project. This
should be as stable as possible so that the team have an up to date and reliable
base to work from. All tests and QA should pass on this branch so [continuous
integrationn](https://en.wikipedia.org/wiki/Continuous_integration) is possible.

### Single Commit Features

If there is a features or minor update that can be made in a single commit then
it is acceptable to commit directly to **develop** but problems can arise if the
feature takes longer than a day to complete. It is important your work is pushed
to origin regularly to protect against loss, so as soon as it is suspected that
a feature is going to take longer than expected, or is going to be a series of
commits, then a branch should be created so the commits can be pushed to
origin without risking pushing partial features to develop.

## Feature Branches

New features should branch from develop, from a release branch or possibly
even a hotfix if something is missed. It is important that you only merge
branches that you want to incorporate into your feature so you do not introduce
other unrelated features when your feature is merged in. It is normally safe to
regularly merge up from the branch you originally branched from.

Naming Convention: feature/{feature-name}

### Examples

    git checkout -b feature/profile-page
    git checkout -b feature/access-control

**Unlike gitflow we recommend that developers push their feature branches to
origin at least daily** even when features are not complete.

    git push origin feature/profile-page

git is distributed by design but at Clock we nearly always have a central
origin, usually github. Pushing your work daily to origin, or at least pushing
commits to a team member, prevents code loss when a developer goes AWOL, is out
of reach or looses their laptop.

## Bug Branches

Often a task is solely to fix a bug. These could be created as feature branches,
but it feels wrong saying **'feature/fixing-wrongly-aligned-panel'** so we have
adopted the **'bug/'** prefix for such cases. **'bug/badly-aligned-button'**

## Release Branches

In most cases release branches are branched from develop once an agreed amount
of completeness is reached. Generally develop is not again merged into the
release branch, but small changes may be merged from other feature branches if
something is missed or a long time elapses between creation and deployment. Once
deployed and all checks pass, the release should be merged back onto develop and
master. As described in gitflow releases are versioned with major and minor at
time of creation.

Naming Convention: release/{version}

### Examples

    git checkout -b release/1.2

## Hotfix Branches

If the commits made to develop are not wanted in a release then a hotfix is
required. Hotfixes are releases that resolve issues and add essential features
outside of the planned release schedule. They should be branched from master and
merged back to master then into develop **and any pending release branches to
prevent regression.**

Naming Convention: hotfix/{version}

### Examples
    git checkout -b hotfix/1.2.1

## Versioning

Before any deployment the release or hotfix branch should be tagged with a version
number. Clock follow the [Semantic Versioning](http://semver.org/) for all
software versioning.

    git tag 1.48.0-rc1
    git push --tags

Deployment to testing for stakeholder to check should be either an
[alpha](http://en.wikipedia.org/wiki/Software_release_life_cycle#Alpha) or
[beta](http://en.wikipedia.org/wiki/Software_release_life_cycle#Beta) version
with the tag: **1.48.0-alpha1**, **1.48.0-beta1**, **1.48.0-beta2**. Alpha can
be any partial feature that you want to simply show some progress of. beta
releases should pass some basic quality assurance and have the potential to
become a release candidate.

The first version of a release to go on staging should be the first
[release candidate](http://en.wikipedia.org/wiki/Software_release_life_cycle#Release_candidate): **1.48.0-rc1** this should then increase, **1.48.0-rc2**, **1.48.0-rc3**,
**1.48.0-rc4** until the release is deployed and signed off, at which point the
last release candidate can be tagged as the final release.

    git tag 1.48.0
    git push --tags

Then merged back on to develop and master.

    git checkout develop
    git pull origin develop
    git merge 1.48.0
    git push origin develop
    git checkout master
    git pull origin master
    git merge 1.48.0
    git push origin master

Hotfixes should increase the [patch version](http://semver.org/)

    git checkout hotfix/ad-rename
    git tag 1.48.1-rc1
    git push --tags

### Pre-golive Releases

Before a project is publicly released, 0.x releases can be made using the same
method as described above.

## Smooth Operator

Even with an agreed process it can be very hard to keep all developers on the
correct branch. Strong project leadership, good team communications and regular
reviews of the 'git log' is required to ensure features are not lost and the
team know where they should be committing and merging their work.

## Release Manager

This process goes a long way to ensure a slick development and release effort,
but don't think that you can get away without assigning every project one person
who is responsible for creating releases, arranging the deployment,
communicating with the team and stakeholders, then ensure the merging down after
deployment. A good release manager will keep the repo healthy and well ordered,
the development team happy and well informed and give a premium customer
experience.