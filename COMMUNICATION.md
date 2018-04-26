# Communicating with your colleaugues

Within our iOS team, we mostly communicate with each other via the magic of the internet, and that's great! It's easy and very efficient, especially since we're often in different cities and/or countries. 

But online communication can easily result into misunderstandings, which tend to be long, time consuming and inefficient.These guidelines are meant to solve issues in our professional communication, like pull request comments, code review and more thorough discussions.

Keep them in the back of your head when commenting on GitHub or Slack.

## Table of contents

- [Short replies](#short-replies)
- [Criticizing or asking questions](#criticizing-or-asking-questions)
- [Giving advice or tips](#giving-advice-or-tips)
- [Clarifications and disagreements](#clarifications-and-disagreements)

## Short replies

When you write a reply of less than ~5 words, have an alarm bell in the back of your head 🔔. Is it just tacit agreement? Then it’s probably fine! “Great job”, “Couldn’t have put it better”,  “totes awesome!” are all fine. 

In every other case though: 🚨! It's good practice to elaborate. It may seem inefficient to be so verbose in our replies, but it will avoid misunderstandings, which can result in far greater inefficiency.

## Criticizing or asking questions

Start by repeating **what** you are replying to in your own words. This forces you to indulge yourself into the perspective of the other, which creates a greater understanding of the issue both for yourself and the other when reading your reply.

### Avoid:

- Don't use optional here.
- What’s your suggestion?

### Prefer:

- **I see you are using an optional as an array of User object in the example above**, have you considered having that property default to an empty array?
- **Okay, so you say using a collectionView here would be over-engineering**, how would you then solve this problem instead?

## Giving advice or tips

Explain why you think this might be helpful in this particular case. Don’t just prescribe, but try to make your case for a particular approach.

### Avoid:

- Why not use a CollectionView for this?
- Read this article:

### Prefer:

- A collection view might be helpful in this case, **because it would take care of the layout for you. Otherwise you’ll be redoing the same work, implementing your own lightweight collection view instead.**
- This article might be interesting for you, **as it explains the threats of storing API keys in plaintext.**

## Clarifications and disagreements

If you feel someone’s reaction is not clear enough (i.e. someone’s reaction confuses you, or even agitates you), ask for a clarification, or if the person could rephrase that, if the meaning isn’t clear. 

Try to keep in mind that in written communication, it’s really easy to misread something or to interpret something out of context. Sometimes something was poorly phrased. Sometimes we’re switching between contexts too often, and end up being shorter than we should. 

However, don't be afraid to let your colleagues know if you feel they're not following these guidelines, or if you're feeling attacked. All humans can use reminders from time to time to be a little nicer to each other. 

Keep in mind that you are often reviewing or discussing something that someone else has worked on really hard, so always be respectful. [These guidelines for giving feedback on pull requests are a good reference](GIT_AND_GITHUB.md#reviewing-pull-requests).

If following these suggestions doesn't clear things up, consider a call to solve the issue if the matter is pressing, or wait for the next iOS meeting to continue discussing it.

For the long term, look at this document together to see what could be improved in the future. If you realize something critical is missing in these guidelines, please add to it.