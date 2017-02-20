# LeanIX Custom Dashboard by AxelSpringer

This project is a custom dashboard for [LeanIX EAM](https://www.leanix.net/en/product/knowledge) which gives an overview on certain Fact Sheets from data gathered form the API. It can be plugged in "out of the box".

![Screenshot](https://raw.githubusercontent.com/leanix/leanix-dashboard-as/master/docs/screen.png)

## Usage / Integration

LeanIX support requires an URL to the custom dashboard. It must match the following requirements:

* accessible via ssl (https://...)
* provide an url to a file if possible to prevent unwanted redirects (e.g. url ends on .../index.html)
* should be publicly accessible on host (having to go through basic auth etc. can be cumbersome for end users)

Example: https://leanix.github.io/leanix-dashboard-as/master/index.html


## Development

### How to run

In the production environment the plugin needs to be enabled by LeanIX support. However, the plugin is an AngularJS app which just needs two query parameters to work: 

* token: an OAuth2 token for authentication (passed automatically by EAM)
* baseUrl: the base url of the workspace to work on (passed automatically by EAM)

### How to build

You need Node installed, and npm (comes with node). 

Download all dependencies with 

`npm install` 

then run

`./node_modules/bower/bin/bower install --config.interactive=false;`

Start the web server with `gulp serve`.

## Deploy a new version on GitHub

* build and ensure to check in the build
* commit and ensure changes go into gh-pages branch (see below)

The code for __"master"__ branch will be published in https://leanix.github.io/leanix-dashboard-as/master.

### using git post-commit hook

* Create a post-commit hook (an executable file under .git/hooks/post-commit):

```bash
#!/bin/sh
branch=$(git rev-parse --abbrev-ref HEAD)
if [ "gh-pages" == "$branch" ]; then
    exit
fi

echo Updating gh-pages for branch $branch

git checkout gh-pages
git checkout $branch -- dist
mkdir -p $branch
git rm --ignore-unmatch -rf $branch
mv -f dist/* $branch
git rm -rf --ignore-unmatch dist
git add $branch
git describe --always | git commit -m -
git checkout $branch
```

* build for production `gulp build`
* commit (hooks runs afterwards)
* `git push` (pushes both your branch and the gh-pages branch)

### without hooks

* build for production `gulp build`
* commit
* run build.sh
* `git push` (pushes both your branch and the gh-pages branch)



## License

This project is MIT-licensed. See http://leanix.mit-license.org/
