# Communicating within the iOS team

## Why we have guidelines about communicating
Within our iOS team we mostly communicate with each other via the magic of the internet, and that is great! It's easy and very efficient. But online communication can easily result into misunderstandings, which tend to be long, time consuming and inefficient. That's why we created some guidelines for how we like to communicate professionally, keep them in the back of your head when commenting on GitHub or Slack.

## When to pay attention to the guidelines
Obviously we're communicating all the time. We can't read this document every time we're having something come out of our mouths or keyboards. That would result in silencing the world 🤐, and thats the opposite of the intention of these guidelines.

- **Only in professional communication**
When making jokes, talking about your favourite tea, or planning a team members birthday party surely a lot of things can go wrong in communication as well. But these guidelines are meant to solve issues in our professional communication, like pull request comments, code review and more thorough discussions.

- **When writing short replies**
When you write a reply of less than ~5 words, have an alarm bell in the back of your head 🔔. Is it just tacit agreement? Then it’s probably fine! “Great job”, “Couldn’t have put it better”,  “toats awesome!” are all fine. In every other case though: 🚨!

It good practice to elaborate. It may seem inefficient to be so verbose in our replies, but it will avoid misunderstandings, which can result in way greater inefficiency.

## Criticising or asking questions
Start by repeating **what** you are replying to in your own words. This forces you to indulge yourself into the perspective of the other, which creates a greater understanding of the issue both for yourself and the other when reading your reply.

### To be avoided:
- Don't use optional here.
- What’s your suggestion?

### Preferred:
- **I see you are using an optional as an array of User object in the example above**, have you considered to have that property default to an empty array?

- **Okay, so you say using a collectionView here would be over-engineering**, how would you then solve this problem instead?

## Giving advice or tips
Explain why you think this might be helpful in this particular case. Don’t just prescribe, but try to make your case for a particular approach.

### Avoid:
- Why not use a CollectionView for this?
- Read this article:

### Prefer:
- A collection view might be helpful in this case, **because it would take care of the layout for you. Otherwise you’ll be redoing the same work, implementing your own lightweight collection view instead.**
- This article might be interesting for you, **as it explains the threats of storing API keys in plaintext.**

## When you feel someone is not communicating clear enough
If you feel someone’s reaction is not clear enough (i.e. someone’s reaction confuses you, or even agitates you), look at these guidelines together to see what could be improved in the future. If you realise something critical is missing in the guidelines, please add to it.

Try to keep in mind that written communication it’s really easy to misread something or to interpret something out of context. Try not to assume rudeness, ill intent or malice. Sometimes something was poorly phrased or misread. Sometimes we’re switching between contexts too often, and end up being shorter than we should. Try asking for clarification, or if the person could rephrase that, if the meaning isn’t clear. 👍

If following the guidelines don't clear things up, consider a call to solve the issue if the matter is pressing, or wait for the next iOS meeting to continue discussing it.

### Good to keep in mind:

When reviewing pull requests keep in mind that you are reviewing something that someone else has worked on really hard, so always be respectful. These guidelines for giving feedback are a good reference:

https://github.com/bakkenbaeck/iOS-playbook/blob/master/GIT_AND_GITHUB.md#reviewing-pull-requests
