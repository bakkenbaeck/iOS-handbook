Every agnostic component (UI control, category, helper, etc) should be created in a separate repository.

Your component should be unit tested and documented.

# Steps to create a component
- [ ] Create repo
- [ ] Download [pod-template](https://github.com/hyperoslo/pod-template/archive/master.zip) or [ui-component-template](https://github.com/hyperoslo/ui-component-template/archive/master.zip) depending on your needs
- [ ] Extract files to your empty repo (if you extract the zip file to somewhere else, like Downloads, be sure to also copy hidden files to your repo)
- [ ] Run `./init.rb` in Terminal.app
- [ ] [Enable Travis for your repository](https://travis-ci.org/profile/hyperoslo). If your component doesn't show up press "Sync Now"
- [ ] To test your pod in another project while developing it you have to make sure all changes of your pod are commited and synced, then make sure `pod '<YOUR_PODS_NAME>', :git => 'https://github.com/hyperoslo/<YOUR_PODS_NAME>.git` is in the other project's Podfile. Running `pod install <YOUR_PODS_NAME>` (first time) or `pod update <YOUR_PODS_NAME>` (after each change to your pod) in the other project is required
- [ ] Add your beautiful code and make a `Initial implementation` pull request
- [ ] Publish your pod by running `pod trunk push`(*)
- [ ] Add Hyper as co-owner by running `pod trunk add-owner <PODNAME> ios@hyper.no`
- [ ] Share it with the world!
- [ ] :cake:

(*) If you don't have a CocoaPods account you can create one by following [this steps](http://guides.cocoapods.org/making/getting-setup-with-trunk.html#getting-started)
