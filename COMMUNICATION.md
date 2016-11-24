# Communicating within the iOS team

## Why we have guidelines about communicating
Within our iOS team we mostly communicate with each other via the magic of the internet, and that is great! It's easy and very efficient. But online communication can easily result into misunderstandings, which tend to be long, time consuming and inefficient. That's why we created some guidelines for how we like to communicate professionally, keep them in the back of your head when commenting on GitHub or Slack'

## When to pay attention to the guidelines
Obviously we're communicating all the time. We can't read this document every time we're having something come out of our mouths or keyboards. That would result in silencing the world 🤐, and thats the opposite of the intention of these guidelines.

- **Only in professional communication**
When making jokes, talking about your favourite tea, or planning a team members birthday party surely a lot of things can go wrong in communication as well. But these guidelines are meant to solve issues in our professional communication, like pull request comments, code review and more thorough discussions.

- **When writing short replies**
When you write a reply of less than ~5 words, have an alarm bell in the back of your head 🔔. Is it just tacit agreement? Then it’s probably fine! “Great job”, “Couldn’t have put it better”,  “toats awesome!” are all fine. In every other case though: 🚨!

It good practice to elaborate. It may seem inefficient to be so verbose in our replies, but it will avoid misunderstandings, which can result in way greater inefficiency. Avoid being pennywise, pound foolish.

## Criticising or asking questions
A good idea is to start by repeating **what** you are replying to in your own words. This avoids cases where due to the high volume of messages, the context of your comment is lost. It also helps you make sure you yourself fully understood the problem, and didn’t misread anything.

### To be avoided:
- Don't use optional here. 
- What’s your suggestion?

### Preferred:
- **I see you are using an optional as an array of User object in the example above**, wouldn’t it be better to have that property default to an empty array?

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

Try to keep in mind that written communication it’s really easy to misread something or to interpret something out of context. Try not to assume rudeness, ill intent or malice. Sometimes something was poorly phrased os misread. Sometimes we’re switching between contexts too often, and end up being shorter than we should. Try asking for clarification, or if the person could rephrase that, if the meaning is’t clear. 👍 

If following the guidelines don't clear things up, consider a call to solve the issue if the matter is pressing, or wait for the next iOS meeting to continue discussing it.

### Good to keep in mind:
These are paraphrased or downright copied over from Github’s pull request guideline:

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
- Use emoji to clarify tone. Compare “well, this sucks” to “well, this sucks 😂.”

[Source: How to write the perfect pull request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

