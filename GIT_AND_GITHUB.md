# Git & GitHub Conventions

These conventions apply to our open-source, internal, and client projects.

## Commits

The `master` branch is the stable branch. It's also used for development
purposes, as to avoid having `dev`, `development`, `staging`, `wip` and others.

You might be wondering: why only `master`? What if I have to rollback to apply
a fix? Well, if you find a bug in a previous release, you can just, checkout the tag,
 fork it, fix it, deploy the new build, and finally, submit a PR to merge it with `master`
  to the sound of Daft Punk.

Commit messages are in the [imperative present tense](http://stackoverflow.com/questions/3580013/should-i-use-past-or-present-tense-in-git-commit-messages), and have no period at the end.

Prefer concise commits over gigantic ones. When writing a concise commit message
is difficult, it may indicate too many unrelated changes.

Feel free to elaborate more on the commit description.

```
// Preferred
Add employee avatar in list of employees

// Not preferred
Added image view to display image for employee in employees table view controller.
```

## Branches

### Naming

Branches are awesome. We use `feature/`, `fix/`, `improve/` and `refactor/` for pseudo namespacing:

```
feature/new-feature
fix/make-it-work
improve/make-it-better
refactor/make-it-awesome
```

Use `--no-ff` when you merge to `master`.

It's okay to just push to `master` if it's a quick fix and you really need
it out the door. Otherwise, make a pull request.

### Pull requests

Pull request descriptions should be concise and well written. The reviwer should
be able to copy this description straight into the release notes, instead of
figuring out what changed or was fixed.

Besides, a description with more information could be helpful, for example:

- If it's a static UI, a screenshot would do. 
- If it's an interaction, a gif would help a lot. A good tool to make gifs is [Licecap](http://www.cockos.com/licecap/).
- If it's a bug, steps to reproduce are very useful to help understanding context.

#### When to merge a pull request?

Before merging a pull request, your code has to be reviewed, so that we can learn from you, 
point out your silly mistakes, and/or post a sufficient amount of gifs. The reviewing
process is important. It is better for you to have another person backing you up. More eyes
can mean less bugs and more consistency throughout.

When the reviewer signs off, confirming that the changes are ready, feel free to 
merge your pull request. Avoid merging before receiving a confirmation, even for 
"final fixes" and "last touches". If several developers are involved in the project,
one confirmation should be enough. You can always submit subsequent pull requests or file an issue after the fact.

As a reviewer, when signing off on an issue, be sure to be as explicit and unambiguous as possible.

After you have merged, you should delete the branch and smile. The smile is critical 
part of the process, so don't forget this.

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
