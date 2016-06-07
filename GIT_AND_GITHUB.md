# Git & GitHub Conventions

These conventions apply to our open-source, internal and client projects.

## Commits

The `master` branch is the stable branch, it's also used for development
purposes so just avoid having `dev`, `development`, `staging`, `wip` and others.

You might be wondering, why only `master`? Why if I have to rollback to apply
a fix? Well, if you find a bug in a previous release you can just checkout the
tag, fork it, fix the bug, deploy the new build, and submit a PR to merge it
with `master`.

Write grammatically correct (i.e. start with a capital) commit messages in [imperative, present tense](http://stackoverflow.com/questions/3580013/should-i-use-past-or-present-tense-in-git-commit-messages).

Prefer concise commits over gigantic ones. When writing a concise commit message
is difficult, it may indicate too many unrelated changes.

Commit summaries shouldn't end with a dot. Descriptions should.

```
// Preferred
Add employee avatar in list of employees

// Not preferred
Added image view to display image for employee in employees table view controller.
```

## Branches

### Naming

Branches are awesome. We use `feature`, `fix`, `improve` and `refactor`:

```
feature/new-feature
fix/make-it-work
improve/make-it-better
refactor/make-it-awesome
```

Use `--no-ff` when you merge to `master`.

### Merging

It's okay to just push to `master` if it's a quick fix and you really need
it out the door. Otherwise, make it a pull request.

### Pull requests

Pull request descriptions should be concise and well written. The merger should
be able to copy this description straight into the release notes instead of
figuring out what changed or was fixed.

Besides a clear description more information could be helpful, for example:

- If is static UI, a screenshot would do. 
- If is an interaction a gif would help a lot. 
- If is a bug, steps to reproduce are useful to understand the context.

#### Who merges the pull request?

Core contributors have the final say on what gets merged into their codebase,
so they get to click the button. That could be you, but if you're working with
someone else it should probably be them. The important thing is that someone else
gets to look it over so we can learn from you, point out your silly mistakes and/or
post the sufficient amount of gifs.

Once the pull request is accepted, the person who merged should delete the branch.

#### Reviewing pull requests

As a reviewer, you should ideally be a core team member and have enough context
to make a thorough review of the committed code, just by reading it. However,
there are times when the pull request is a bit on the large side, or it is
referencing existing code not found in the PR, or you for some other reason do
not have a good enough context to review anything else than code style and typos.
In these cases, we encourage you to physically (or virtually) approach the PR
submitter and go through the pull request together.

Be warned, though: By doing this, you will both need to explain your thoughts and
discuss other alternatives. Of course, this means you are putting yourself at risk
of sharing your knowledge and/or learning some new stuff, so please be careful not
to end up being even more awesome than you are.

#### My pull request depends on another pending pull request. What to do?

If your next pull request is really that intimately related to your last,
consider to continue working on it instead of opening another.

If it really makes sense to open another, it's okay to base your next pull
request on your unmerged branch, just make sure to make it clear in the PR description.
Finally make sure to merge any changes you've made to its parent during its review.

[_This is partly based on Hyper's Git and GitHub guide._](https://github.com/hyperoslo/playbook/blob/master/GIT_AND_GITHUB.md)
