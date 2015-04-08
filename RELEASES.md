# Releases

Steps for releasing a new version of your project:

1. Switch to the `develop` branch
2. Bump the projects version number (make sure to use [semantic versioning](http://semver.org/))
3. Create and upload the archive to HockeyApp
4. Commit and push your changes to the `develop` branch
5. Create a [release on GitHub](https://help.github.com/articles/creating-releases/)
6. If the release uses the `production` server, then merge your changes to the `master` branch, too
