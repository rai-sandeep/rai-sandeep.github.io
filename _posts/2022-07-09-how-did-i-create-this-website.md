---
title: How did I Create this Blog
date: 2022-07-10 16:40:00 +0530
categories: [Blogging, Tutorial]
tags: [jekyll, github-pages]
---

This is a step by step guide on how I created this blog and how you can create yours too.
Most of the content here is sourced from the [Chirpy website](https://chirpy.cotes.page).

Before I start, let me explain why I chose [GitHub Pages](https://pages.github.com) and [Jekyll](https://jekyllrb.com/) with [Chirpy](https://github.com/cotes2020/jekyll-theme-chirpy) theme. 
If you are convinced already and want to skip to the tutorial, click [here](#creating-the-site-repository).

## Why GitHub Pages

What's better than having the source code for your blog on GitHub with the convenience of automated deploys! It makes even more sense to have a tech focused blog hosted on GitHub (that too for free). 

## Why Jekyll with Chirpy

[GitHub pages website](https://pages.github.com) has a [Blogging with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll) article, which is convincing enough. For me, one look at the [Chirpy website](https://chirpy.cotes.page) was all it needed to fall in love. If my blog can look like that, what's stopping me?

## Creating the Site Repository

Create a new repository from the [**Chirpy Starter**](https://github.com/cotes2020/chirpy-starter/generate) and name it `<GH_USERNAME>.github.io`, where `GH_USERNAME` represents your GitHub username.

The repository I created is [rai-sandeep.github.io](https://github.com/rai-sandeep/rai-sandeep.github.io).

## Installing Jekyll

Follow the instructions in the [Jekyll Docs](https://jekyllrb.com/docs/installation/).
The page has detailed instructions for different operating systems. 

If you are able to install everything successfully, skip to [Installing Dependencies](#installing-dependencies).

### Troubleshooting on Mac

I followed the [guide for macOS](https://jekyllrb.com/docs/installation/macos/), which lists the below terminal commands. 

```console
$ brew install chruby ruby-install
$ ruby-install ruby
$ echo "source $(brew --prefix)/opt/chruby/share/chruby/chruby.sh" >> ~/.zshrc
$ echo "source $(brew --prefix)/opt/chruby/share/chruby/auto.sh" >> ~/.zshrc
$ echo "chruby ruby-3.1.2" >> ~/.zshrc
$ ruby -v  
$ gem install jekyll
```

You need only these commands if your Homebrew and xcode command line tools are perfectly setup. 

#### Homebrew issues

Before you start, run `brew update` and `brew doctor` to validate Homebrew. If it complains about shallow homebrew-core, run the unshallow command it suggests (`git -C /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core fetch --unshallow` in my case). 

#### Ruby issues

If ruby installation  complains about C libraries, make sure xcode command line is installed and run `xcode-select --install` if needed. 

## Installing Dependencies

Once jekyll is installed, clone your repository, navigate to its root directory and install dependencies with the `bundle` command.

```console
$ bundle
```

## Configuration

Update the variables of `_config.yml`{: .filepath} as needed. These are the variables I updated:

- `timezone`
- `title`
- `tagline`
- `description`
- `github: username`
- `twitter: username`
- `social: name, email, links`
- `avatar`
- `comments: active, giscus`

See my `_config.yml`{: .filepath} [here](https://github.com/rai-sandeep/rai-sandeep.github.io/blob/main/_config.yml).

### Avatar

Place your picture or avatar in a directory and use it. Example: `avatar: '/assets/images/sandeep.jpeg'`

### Comments

If you want to enable comments on your posts, `disqus`, `utterances` and `giscus` are the supported options.

#### Giscus

I chose [**Giscus**](https://giscus.app), a comments system powered by [GitHub Discussions](https://docs.github.com/en/discussions). 
Head over to the **Configuration** section on their [website](https://giscus.app) for instructions. 
See below for the steps I followed.
1. Create a public repository similar to [this](https://github.com/rai-sandeep/giscus).
2. Install [giscus app](https://github.com/apps/giscus).
3. Enable [Discussions feature](https://docs.github.com/en/github/administering-a-repository/managing-repository-settings/enabling-or-disabling-github-discussions-for-a-repository) for your repository.
4. On the [website](https://giscus.app), enter the repository and set **Discussion Category** as `General`.
5. The **Enable Giscus** section should now show all the details needed for the `_config.yml`{: .filepath}. See my configuration under `comments` [here](https://github.com/rai-sandeep/rai-sandeep.github.io/blob/main/_config.yml).

### About page

Add details about yourself in the `_tabs/about.md`{: .filepath} file ([example](https://github.com/rai-sandeep/rai-sandeep.github.io/blob/main/_tabs/about.md)).

## Running Local Server

You may want to preview the site contents before publishing, so just run it by:

```console
$ bundle exec jekyll s
```

After a while, the local service will be published at _<http://127.0.0.1:4000>_.

## Deployment

Push your changes to `<GH_USERNAME>.github.io` on GitHub.

If you have committed `Gemfile.lock`{: .filepath} to the repo, and your runtime system is not Linux, don't forget to update the platform list in the lock file:

  ```console
  $ bundle lock --add-platform x86_64-linux
  ```

After the above steps, rename your repository to `<GH_USERNAME>.github.io` on GitHub.

Now publish your Jekyll site by:

1. Push any commit to remote to trigger the GitHub Actions workflow. Once the build is complete and successful, a new remote branch named `gh-pages` will appear to store the built site files.

2. Browse to your repository on GitHub. Select the tab _Settings_, then click _Pages_ in the left navigation bar, and then in the section **Source** of _GitHub Pages_, select the `/(root)` directory of branch `gh-pages` as the [publishing source](https://docs.github.com/en/github/working-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site). Remember to click <kbd>Save</kbd> before leaving.

3. Visit your website at the address indicated by GitHub.

## Write your first post

Congratulations! Your blog should now be online.

The obvious next step is to add a post similar the one you are reading now ([source code](https://github.com/rai-sandeep/rai-sandeep.github.io/tree/main/_posts/2022-07-09-how-did-i-create-this-website.md)). Follow [instructions on the Chirpy website](https://chirpy.cotes.page/posts/write-a-new-post/) and you should be set. 