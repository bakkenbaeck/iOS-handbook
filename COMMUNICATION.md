# Communicating within the iOS team

## Why we have guidelines about communicating
Within our iOS team we moslty communicate with eachother via the magic of the internet, and that is great! It's easy and very efficient. But online communication can easily result into misunderstandings, which tend to be long, time consuming and inefficient. That's why we created some guidelines for how we like to communicate professionaly, keep them in the back of your head when commenting on GitHub or Slack'

## When to pay attention to the guidelines
Obviously we're communicating all the time, we can't read this article every time we're having something come out of our mouths or keyboards. That would result in silencing the world 🤐, and thats the opposite of the intention of these guidelines.

> - **Only in professional communication**
When making jokes, talking about your favorite tea, or planning a team members birthday party surely a lot of things can go wrong in communication as well. But these guidelines are meant to solve issues in our professional comunication.
> - **When writing short replies**
When you write a reply of less than ~5 words, have an alarmbel in the back of your head 🔔  Do you agree on what you are replying to? Then it’s probably fine! “Great job”, “Couldn’t have put it better”,  “toats awesome!” In every other case though: 🚨  It is probably necessary to elaborate your reply according to the guidelines below. It may seem inneficient to be so verbose in our reply, but it will avoid misunderstandings which can result in way greater inneficiency.


## Giving critique or asking a question 
Start your reply with repeating **what** you are replying on in your own words.

### So not:
>- Don't use optional here. 
>- What’s your suggestion?

### But:
>- **I see you are using a optional as an array of User object in the example above**, wouldn’t it be better to initialise the class with that user object?
>- **Oke, so you say using a collectionView here would be overengineering**, how would you then suggest to solve the problem im pointing out in this PR?

## Giving advice or tips
Explain why you think this might be helpful in this particular case.

### So not:
>- Why not use a CollectionView for this?
>- Read this article:

### But:
>- A collectionView might be helpful in this case, **because it would take care of the layout for you.**
>- This article might be interesting for you, **it explains the threats of storing data unencrypted.**

## When you feel someone is not communicating clear enough
If you feel someone’s reaction is not clear enough (i.e. someone’s reaction confuses you, or even agitates you) look at these guidelines together with that person to see what could be improved in the future. If you realise something critical is missing in the guidelines, please add to it.

### Good to keep in mind:
These are the guidelines from GitHub for giving feedback on a PR, there's some valuable points in it for online communication in general:'

> - Familiarize yourself with the context of the issue, and reasons why this Pull Request exists.
> - If you disagree strongly, consider giving it a few minutes before responding; think before you react.
> - Ask, don’t tell. (“What do you think about trying…?” rather than “Don’t do…”)
> - Explain your reasons why code should be changed. (Not in line with the style guide? A personal preference?)
> - Offer ways to simplify or improve code.
> - Avoid using derogatory terms, like “stupid”, when referring to the work someone has produced.
> - Be humble. (“I’m not sure, let’s try…”)
> - Avoid hyperbole. (“NEVER do…”)
> - Aim to develop professional skills, group knowledge and product quality, through group critique.
> - Be aware of negative bias with online communication. (If content is neutral, we assume the tone is negative.) Can you use positive language as opposed to neutral?
> - Use emoji to clarify tone. Compare “✨ ✨ Looks good 👍 ✨ ✨” to “Looks good.”

[Source: How to write the perfect pull request](https://github.com/blog/1943-how-to-write-the-perfect-pull-request)

