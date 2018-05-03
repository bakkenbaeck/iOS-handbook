# Git & GitHub Conventions

These conventions apply to our open-source, internal, and client projects.

## Commits

The `master` branch is the stable branch. It's also used for development purposes, as to avoid having `dev`, `development`, `staging`, `wip` and others.

You might be wondering: why only `master`? What if I have to rollback to apply a fix? Well, if you find a bug in a previous release, you can just, checkout the tag, fork it, fix it, deploy the new build, and finally, submit a PR to merge it with `master` to the sound of Daft Punk.

Commit messages are in the [imperative present tense](http://stackoverflow.com/questions/3580013/should-i-use-past-or-present-tense-in-git-commit-messages), and have no period at the end.

Prefer concise commits over gigantic ones. When writing a concise commit message is difficult, it may indicate too many unrelated changes.

```
// Preferred
Add employee avatar in list of employees

// Not preferred
Added image view to display image for employee in employees table view controller.
```

If you need to elaborate more, you can do so in the commit description.

## Branches

Branches are awesome. We use `feature/`, `fix/`, `improve/` and `refactor/` for pseudo-namespacing:

```
feature/new-feature
fix/make-it-work
improve/make-it-better
refactor/make-it-awesome
```

Rarely, it's okay to just push to `master` if it's a quick fix and you really need it out the door. Usually, you make a pull request to `master` or to a long-running `feature` branch instead.

### Pull requests

Pull request descriptions should be concise and well written. The reviewer should be able to copy this description straight into the release notes, instead of figuring out what changed or was fixed.

Besides, a description with more information could be helpful, for example:

- If it's a static UI, a screenshot will do. 
- If it's an interaction, a `gif` would help a lot. Good tools to make `gif`s include [Licecap](http://www.cockos.com/licecap/), [GIPHY Capture](https://itunes.apple.com/us/app/giphy-capture-the-gif-maker/id668208984?mt=12), and [GIF Brewery](http://gifbrewery.com/).
- If it's a bug, steps to reproduce are very useful to help understanding context.

Before merging a pull request, your code has to be reviewed, so that we can learn from you, help you find mistakes, and/or post a sufficient amount of gifs. The reviewing process is important. It is better for you to have another person backing you up. More eyes on code mean fewer bugs and better consistency throughout.


#### Reviewing pull requests

As a reviewer, you should ideally be a core team member and have enough context to make a thorough review of the committed code, just by reading it. However, there are times when the pull request is a bit on the large side, or it is referencing existing code not found in the PR, or you for some other reason do not have a good enough context to review anything else than code style and typos. In these cases, we encourage you to physically (or virtually) approach the PR submitter and go through the pull request together.

Be warned, though: By doing this, you will both need to explain your thoughts and discuss other alternatives. Of course, this means you are putting yourself at risk of sharing your knowledge and/or learning some new stuff, so please be careful not to end up being even more awesome than you already are.

As a reviewer your job is to be the extra pair of eyes, so the code will get twice as good. You'll look at things such as code style and structure. You'll discuss things that you might've done differently, point out possible more elegant solutions based on something you've learned, and so on. The goal here is to make the whole team better in the process.

Keep in mind that you are reviewing something that someone else has worked on really hard, so always be respectful. These guidelines for giving feedback are a good reference:

- Familiarise yourself with the context of the issue.
- If you disagree strongly, consider giving it a few minutes before responding; think before you react. Try to empathise.
- Ask, don’t tell. (“What do you think about trying…?” rather than “Don’t do…”)
- Explain your reasons why code should be changed. (Not in line with the style guide? A personal preference?)
- Offer ways to simplify or improve code.
- Avoid using derogatory terms, like “stupid”, when referring to the work someone has produced. Don’t shame, no one started out knowing.
- Be humble. (“I’m not sure, let’s try…”)
- Never deal in absolutes. Unless needed. (“We should never store sensitive data in plain text.”)
- Aim to develop professional skills, group knowledge and product quality, through group critique. That goes both way: try to be open and welcoming to criticism.
- Be aware of negative bias with online communication. (If content is neutral, we assume the tone is negative.) Try to use positive language instead.
- Use emoji to clarify tone. Compare “ok” to “ok 😃.”

[Based on GitHub's "How to write the perfect pull request".](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

If everything is fine, feel free to merge the pull request. Avoid merging before receiving a feedback. If several developers are involved in the project, one confirmation should be enough. You can always submit subsequent pull requests or file an issue after the fact.

After you have merged, you should delete the branch and smile. The smile is critical part of the process, so don't forget this.

#### My pull request depends on another pending pull request. What to do?

If your next pull request is really that intimately related to your last,
consider to continue working on it instead of opening another.

If it really makes sense to open another, it's okay to base your next pull request on your unmerged branch, just make sure to make it clear in the PR description. Note in the description that the PR depends on its parent PR, and add a link to the pending parent PR. 

Finally, make sure to merge any changes you've made to its parent during its review before seeking final review of your child PR.

[_This is partly based on Hyper's Git and GitHub guide._](https://github.com/hyperoslo/playbook/blob/master/GIT_AND_GITHUB.md)

## Releases

Steps for releasing a new version of your app:

1. Switch to the `master` branch.
2. Bump the projects version number (make sure to use [semantic versioning](http://semver.org/)) and build number.
3. Create the archive.
4. Upload the archive to iTunes Connect for submission to TestFlight or App Review.
4. Create a [release on GitHub](https://help.github.com/articles/creating-releases/). Remember to mark it as a `pre-release` if needed.
