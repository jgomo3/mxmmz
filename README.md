# MxMMz

This repository contains the code to generate my website. It contains:

- The content
- The styling
- The scripts to develop & generate the website

## Setup

To work with this code you need these dependencies:

- Babashka: `https://github.com/borkdude/babashka#installation`
- Optionally: Babashka filewatcher pod:
  `https://github.com/babashka/pod-babashka-filewatcher`

Once those are installed and working, you can develop and generate the website.

To generate the website, call the `gen.clj` script from the rootfolder. It will
generate the website in a `publish` folder, ready to be published.

To develop the website, call the `dev.clj` script from the rootfolder. It will
start a watcher that will call the `gen.clj` script whenever a file is saved.

## Publish

For publishing I'm basically using this setup:
https://medium.com/linagora-engineering/deploying-your-js-app-to-github-pages-the-easy-way-or-not-1ef8c48424b7

It depends on some 'configuration magic' and because the web is a rough place
these days (and websites may go down or page URLs might change) I'm going to
repeat (and extend) the procedure here.

Some preconditions:

- Make sure you have github pages up and running, instructions here:
  https://pages.github.com
- Now the big leap (for me) was to understand that you don't have to publish
  everything you want to publish via that particular github pages repository!
  - So while you need a `username.github.io` repository, you can still publish
    pages from another repository.
  - If that "other-repository" is called `other-repository`, then the
    `index.html` in that other repository will be published on
    `https://username.github.io/other-repository`
  - ... that is ... if you configure it to be published in the settings of
    `other-repository`
  - This can be done in settings -> options -> github pages where you'll have to
    point to the branch where the index.html can be found that github pages has
    to publish.
- To make it a little bit more convenient, the setup described in the medium
  post
  - publishes from the gh-pages branch
  - makes a setup where you don't have to change branches to publish the
    website. Simply work on `master` or `develop`, at some point run your
    publish script and then `cd` to the `publish` directory (or in the post the
    `dist` directory) to automagically end up in the gh-pages branch where the
    only thing to do is add, commit and push your newly generated website
- WARNING: the setup assumes:
  - you start off with a repository where there is no `publish` directory yet
  - where `publish/` is not yet added to the `.gitignore` file
  - where you run the commands in the root folder of an initialized git
    repository:

``` bash
# Create an orphan branch named gh-pages
git checkout --orphan gh-pages

# Remove all files from staging
git rm -rf .

# Create an empty commit so that you will be able to push on the branch next
git commit --allow-empty -m "Init empty branch"

# Push the branch
git push origin gh-pages

# Come back to master
git checkout master

# Add publish to .gitignore
echo "publish/" >> .gitignore

git worktree add publish gh-pages
```

That's it. Now you run `./gen.clj` and whenever you think you're ready to
publish, `cd publish` and add, commit, push. `cd..` and you're back on your
original branch.

## Motivation

- It's time for me to have my own little space on the internet to share my own
  little thoughts and coding experiments.
- I have created some blogs before via wordpress, gatsby, multiple markdown
  templating engines, etc. but the overhead is often too big for me. When I want
  to write, I want to write, and I don't want to 'negotiate' with broken
  dependencies, unclear documentation, or "super-simple" config file structure
  (that somehow always grows into a turing-complete DSL, often without backwards
  compatibility).
- I'm not afraid of html and want to learn (more about) CSS.
- Constraints bring out creativity, so therefore I've constrained myself to only
  use clojure files (apart from the `README.md` and the `.gitignore` files).

## Design decisions

- use the least possible dependencies
- only use clojure files
- stay close to standards so if my sparse dependencies break I still have
  options
  - use hiccup style html (https://github.com/weavejester/hiccup)
  - use hiccup style css (https://github.com/noprompt/garden)
- Create (interactive) code examples (and their tests) in other repositories and
  only include the generated code in this repository, so this repository stays
  clean.

## Credits

- https://github.com/borkdude really helped me, not only with his awesome
  https://github.com/borkdude/babashka tool, but also with this particular
  setup. Thanks Michiel!

## License

- Use the code however you want at your own risk. It is not copyrighted.
- The posts are my work and subject to normal copyright law.
