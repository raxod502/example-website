# Example website

This is a [static
site](https://www.netguru.com/blog/what-are-static-site-generators)
set up similarly to my [personal
website](https://intuitiveexplanations.com/), but much simpler. It
should be suitable as a starting point for anyone to create their own
personal website. Of course, the appearance can be modified by telling
[Jekyll](https://jekyllrb.com/) to use a different
[theme](https://jekyllthemes.io/). This may require more effort than
it should, due to issues with layout includes (see below), but it
should be totally feasible.

## Software stack

The content is written in
[Markdown](https://www.markdownguide.org/getting-started/), which is
transformed into web pages by [Jekyll](https://jekyllrb.com/). The
content is stored on GitHub, which automatically handles deployment to
[GitHub Pages](https://pages.github.com/).

### Differences from my personal website

This section is only for your curiosity, and is completely irrelevant
to usage of the example website setup.

The main difference with my personal website is that it has a much
more complicated build system, because it requires a compilation step.
My website contains not only Markdown content, but also LaTeX which is
compiled during deployment as well as some image files which are
translated to another format by GIMP. The compilation happens on
CircleCI and then deployment goes is on Netlify. Some portions of my
website are closed-source, and these are pulled in optionally as Git
submodules. Compilation of different parts of the website is organized
using a Makefile, and I have a number of scripts which are used to
sandbox the entire thing inside Docker to isolate dependencies. I use
a fork of Jekyll to work around an upstream bug relating to symlink
handling during the static file copying step. There is some custom
MathJax configuration to do font matching, and I overrode some more
layouts and partials to set up Fathom Analytics and other such things.

## Project setup

First install the dependencies (installation procedure varies by
operating system and/or distribution):

* [Ruby](https://www.ruby-lang.org/en/) including
  [Bundler](https://bundler.io/) which is used to install Jekyll
* Dependencies for Jekyll:
    * [GCC](https://gcc.gnu.org/)
    * [Make](https://www.gnu.org/software/make/)
    * Ruby development headers (typically installed automatically with
      Ruby, except on Linux)

Then use Bundler to install Jekyll:

    $ bundle install

Now you can build and run the website locally:

    $ bundle exec jekyll serve

Navigate to <http://localhost:4000> to see it. (With the default
configuration the site will be instead at
<http://localhost:4000/example-website/>.)

Changes to site content will be reflected automatically as soon as you
refresh the page on your browser. However, changes to `_config.yml`,
which controls information such as the pages shown in the navbar, will
require you to ctrl-c and restart the `jekyll serve` command.

## Files

    index.md
    about.md
    example/index.md
    example/subpage.md

Site content (name of the file is the URL it will use on the website;
the special name `index` is used to create a page with a trailing
slash, traditionally used for overviews or listings).

    _config.yml

Website configuration, including name, description, URL (this must be
updated when you deploy the website to your own GitHub Pages
location), and the pages displayed in the navbar at the top:

(Note that you can use a [custom
domain](https://docs.github.com/en/github/working-with-github-pages/configuring-a-custom-domain-for-your-github-pages-site)
that you own, instead of the free GitHub Pages `you.github.io` domain.
In this case it should be updated in `_config.yml`.)

    404.html

Source code for page that is displayed when the user goes to a page
that doesn't exist.

    _site

The generated site (i.e. the HTML that was created based on the
Markdown content).

    Gemfile
    Gemfile.lock

Files that are used by Bundler to describe the dependencies (i.e.,
Jekyll) of the website. Read about [Bundler](https://bundler.io/) to
learn how to edit them.

    .bundle/config
    .bundle/gem

Internal files used by Bundler to manage installation.

    _includes/header.html
    _includes/head.html

Source code used to customize how parts of the website are generated.
Hopefully these don't need to be changed. They are copied from the
source code of the Jekyll theme in use,
[Minima](https://github.com/jekyll/minima), and then slightly modified
as follows:

* Add [MathJax](https://www.mathjax.org/) (`head.html`)
* Change navbar to be controlled by an explicit list in `_config.yml`
  rather than automatically listing every single page on the website
  (which obviously would be a mess with more than five pages)
  (`header.html`).

If you change to a different theme, three steps are required: change
`_config.yml`, update the Bundler dependencies, and then copy the
relevant include files from the new theme and apply the same changes
as mentioned above (maybe one or both won't be needed, depending on
which theme is selected).

    .git
    .gitignore

Used by the version control system, [Git](https://git-scm.com/).

    .jekyll-cache

Junk, automatically generated.
