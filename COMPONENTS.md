Every agnostic component (UI control, category, helper, etc) should be created in a separate repository and in Swift.

Your component should be unit tested and documented.

# Steps to create a component
- [ ] Create repo on GitHub
- [ ] Go to your Projects folder
- [ ] Run `git clone https://github.com/hyperoslo/pod-template <PODNAME>`
- [ ] Run `cd <PODNAME>`
- [ ] Run `./init.rb <PODNAME>`
- [ ] [Enable Travis for your repository](https://travis-ci.org/profile/hyperoslo). If your component doesn't show up press "Sync Now"
- [ ] To test your pod in another project while developing it you have to make sure all changes of your pod are commited and synced, then make sure `pod '<YOUR_PODS_NAME>', :git => 'https://github.com/hyperoslo/<YOUR_PODS_NAME>.git` is in the other project's Podfile. Running `pod install <YOUR_PODS_NAME>` (first time) or `pod update <YOUR_PODS_NAME>` (after each change to your pod) in the other project is required
- [ ] Add your beautiful code and make a `Initial implementation` pull request
- [ ] Publish your pod by running `pod trunk push`(*)
- [ ] Add Hyper as co-owner by running `pod trunk add-owner <PODNAME> ios@hyper.no`
- [ ] :cake:

(*) If you don't have a CocoaPods account you can create one by following [this steps](http://guides.cocoapods.org/making/getting-setup-with-trunk.html#getting-started)

# Steps to ship your component

Before shipping your component make sure that the README states why your thing is different or what makes it different than the other ones, been made by you might not be enough. It's very important that your README is :star2: fabulous :star2:.

Make sure to include a super simple example on how to get up and running, if you're using any Pods as dependencies write why do you need those Pods and what do they do. Don't assume that people know everything.

If it's a visual component include a `.gif` showing how does it work or what does it does. It helps a lot for people to understand your Pod without cloning, building and running your project.

- [ ] Make sure to have a cool logo, you can ask [@hyperoslo/design](https://github.com/orgs/hyperoslo/teams/design) to give you a hand
- [ ] Submit it to [Cocoa Controls](https://www.cocoacontrols.com/)
- [ ] Make a PR to [iOS Goodies](https://github.com/iOS-Goodies/iOS-Goodies)
- [ ] Submit your component to [Hacker News](https://news.ycombinator.com/), your post should start with **"Show HN"**
 
**Send a few tweets from the @hyperoslo account**

It's important that you write as part of the group, never use **"I did this"**, use **"We did this"** instead. Take the time to compose all the tweets first and get someone to look at them, prefer single crafted tweets for every person instead of copy pasting one to everybody, it's very annoying and feels spammy.

A few examples:   
https://twitter.com/hyperoslo/status/585126444331835393, https://twitter.com/hyperoslo/status/585123729610510339, https://twitter.com/hyperoslo/status/585123389343424513, https://twitter.com/hyperoslo/status/585120710940557312, https://twitter.com/hyperoslo/status/585119588075044864

- [ ] Send a tweet from the [@hyperoslo](https://twitter.com/hyperoslo) account with a tiny summary and attach the logo of the Pod
- [ ] Send a tweet to [@maniacdev](https://twitter.com/maniacdev)
- [ ] Send a tweet to [@daveverwer](https://twitter.com/daveverwer) and [@iOSDevWeekly](https://twitter.com/iOSDevWeekly)
- [ ] Send a tweet to [@iOSGoodies](https://twitter.com/iOSGoodies) with the link to your PR
- [ ] Send a tweet to [@NatashaTheRobot](https://twitter.com/NatashaTheRobot)
