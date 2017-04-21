# Git & GitHub Conventions

These conventions apply to our open-source, internal, and client projects.

## Commits

The `master` branch is the stable branch. It's also used for development
purposes, as to avoid having `dev`, `development`, `staging`, `wip` and others.

You might be wondering: why only `master`? What if I have to rollback to apply
a fix? Well, if you find a bug in a previous release, you can checkout the tag,
fork it, fix it, deploy the new build, and finally, submit a PR to merge it with `master`
to the sound of Daft Punk.

Commit messages should be consise and descriptive. If writing a concise commit message
is difficult, it may be indicative that too many unrelated changes are bundled together. If that's the case, split it into smaller commits.

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

Sometimes it's okay to just push to `master` if it's a quick fix and you really need
it out the door. Otherwise, make a pull request.

### Pull requests

Pull request descriptions should be concise and well written. The reviewer should
be able to copy this description straight into the release notes, instead of
figuring out what changed or was fixed.

Besides, a description with more information could be helpful, for example:

- If it's a static UI, a screenshot would do. 
- If it's an interaction, a gif would help a lot. A good tool to make gifs is [Licecap](http://www.cockos.com/licecap/).
- If it's a bug, steps to reproduce are very useful to help understanding context.

Before merging a pull request, your code has to be reviewed, so that we can learn from you, 
point out your silly mistakes, and/or post a sufficient amount of gifs. The reviewing
process is important. It is better for you to have another person backing you up. More eyes
can mean less bugs and more consistency throughout.

#### Code Linting

We try to keep our whitespace and code style consistent, one of the tools we use to achieve this is [swiftformat](https://github.com/nicklockwood/SwiftFormat). We recommend installing it using [homebrew](https://brew.sh), for easy running and updating. For most projects you can run it from the root folder, but for projects using Cocoapods/Carthage, it’s recommended to run it from the top subfolder (usually has the same name as the project), to avoid reformatting third-party code.

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
to end up being even more awesome than you already are.

As a reviewer your job is to be the extra pair of eyes, so the code will get twice 
as good. You'll look at things such as code style and structure. You'll discuss things 
that you might've done differently, point out possible more elegant solutions based 
on something you've learned, and so on. The goal here is to make the whole team better 
in the process.

Keep in mind that you are reviewing something that someone else has worked on really 
hard, so always be respectful. These guidelines for giving feedback are a 
good reference:

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

If everything is fine feel free to merge the pull request. Avoid merging before receiving
a feedback. If several developers are involved in the project, one confirmation should be 
enough. You can always submit subsequent pull requests or file an issue after the fact.

After you have merged, you should delete the branch and smile. The smile is critical 
part of the process, so don't forget this.

#### My pull request depends on another pending pull request. What to do?

If your next pull request is really that intimately related to your last,
consider to continue working on it instead of opening another.

If it really makes sense to open another, it's okay to base your next pull
request on your unmerged branch, just make sure to make it clear in the PR description.
Finally make sure to merge any changes you've made to its parent during its review.

[_This is partly based on Hyper's Git and GitHub guide._](https://github.com/hyperoslo/playbook/blob/master/GIT_AND_GITHUB.md)
