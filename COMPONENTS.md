Every agnostic component (UI control, category, helper, etc) should be created in a separate repository.

Your component should be unit tested and documented.

# Steps to create a category component
- [ ] Create repo
- [ ] Download https://github.com/hyperoslo/pod-template/archive/master.zip
- [ ] Extract files to your empty repo
- [ ] Replace all ocurrences of `<PODNAME>`
- [ ] Rename `PODNAME.podspec`
- [ ] Enable Travis support for your repo in https://travis-ci.org/profile/hyperoslo
- [ ] Publish your pod by running `pod trunk push`(*)
- [ ] Add Hyper as co-owner by running `pod trunk add-owner <PODNAME> ios@hyper.no`
- [ ] Share it with the world!
- [ ] :cake:

(*) If you don't have a CocoaPods account you can create one by following [this steps](http://guides.cocoapods.org/making/getting-setup-with-trunk.html#getting-started)
