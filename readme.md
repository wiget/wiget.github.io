![Build Status](https://gitlab.com/pages/nikola/badges/master/build.svg)

---

Example [nikola] website using GitLab Pages.

Learn more about GitLab Pages at https://pages.gitlab.io and the official
documentation https://docs.gitlab.com/ce/user/project/pages/.

---

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [GitLab CI](#gitlab-ci)
- [Building locally](#building-locally)
- [GitLab User or Group Pages](#gitlab-user-or-group-pages)
- [Did you fork this project?](#did-you-fork-this-project)
- [Troubleshooting](#troubleshooting)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## GitLab CI

This project's static Pages are built by [GitLab CI][ci], following the steps
defined in [`.gitlab-ci.yml`](.gitlab-ci.yml):

```
image: paddyhack/nikola

test:
  script:
  - nikola build
  except:
  - master

pages:
  script:
    - nikola build
  artifacts:
    paths:
    - public
  only:
  - master
```

This basically means: use the `paddyhack/nikola` image, which will install Nikola with all its features (`"nikola [extras]"`), and build the site.

## Building locally

To work locally with this project, you'll have to follow the steps below:

1. Fork, clone or download this project
1. [Install][] Nikola
1. Generate the website: `nikola build`
1. Preview your project: `nikola serve`
1. Add content

Read more at Nikola's [documentation][].

## GitLab User or Group Pages

To use this project as your user/group website, you will need one additional
step: just rename your project to `namespace.gitlab.io`, where `namespace` is
your `username` or `groupname`. This can be done by navigating to your
project's **Settings**.

Read more about [user/group Pages][userpages] and [project Pages][projpages].

## Using a different branch

If you keep code on the `master` branch and want the website on a different one, 
for example a `blog` branch, then you must make the corresponding change
on the `pages` job on the `.gitlab-ci.yml` file.

```
  only:
  - blog
```


## Did you fork this project?

If you forked this project for your own use, please go to your project's
**Settings** and remove the forking relationship, which won't be necessary
unless you want to contribute back to the upstream project.

## Troubleshooting

1. CSS is missing! That means two things:

    Either that you have wrongly set up the CSS URL in your templates, or
    your static generator has a configuration option that needs to be explicitly
    set in order to serve static assets under a relative URL.
    
1. Building passes but deploy stage fails.

    Nikola's default configuration will by build the site on the `output` directory,
    but Gitlab expects it on  `public`.  So you must change
    `OUTPUT_FOLDER = "public"` on `conf.py` or deploying will fail.
    
    Alternatively, you can add `mv output public` on the `.gitlab-ci.yml` file  
    after the `nikola build` line.
    
    If you cloned this project as starting point, then `conf.py` is already updated.
    
1. I get a strange lexer exception 

    ![Build fails](https://i.imgur.com/e5nJVct.png)
    You are likely using extensions not enabled by the `paddyhack/nikola`.
    For example, if your site has Ipython/Jupyter posts 
    (that is, `.ipynb` format via `POSTS` or `PAGES` on `conf.py` )
    Gitlab build won't be able to compile them, even if you locally can.
    
    The `paddyhack/nikola` image has the full `nikola[extras]`  but not
    additional software (like ipython, pandoc, latex, or any software you may
    have in your local system).
    
    The fix is to install any extra needed software before building.  
    In the case of `.ipynb` support,  edit the `.gitlab-ci.yml` file and change

```
pages:
    script: 
      - pip3 install jupyter
      - nikola build
```
    
----

Forked from @sukiletxe

[ci]: https://about.gitlab.com/gitlab-ci/
[nikola]: https://getnikola.com/
[install]: https://getnikola.com/getting-started.html
[documentation]: https://getnikola.com/documentation.html
[userpages]: https://docs.gitlab.com/ce/user/project/pages/introduction.html#user-or-group-pages
[projpages]: https://docs.gitlab.com/ce/user/project/pages/introduction.html#project-pages