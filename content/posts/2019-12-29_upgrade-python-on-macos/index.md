---
title: "Upgrade Python on MacOS"
author: "Russ Mckendrick"
date: 2019-12-29T16:21:00.000Z
lastmod: 2021-07-31T12:44:17+01:00

description: ""

subtitle: ""
tags:
 - Macos
 - Python

cover:
    image: "/img/2019-12-29_upgrade-python-on-macos_0.jpeg" 
images:
 - "/img/2019-12-29_upgrade-python-on-macos_0.jpeg"
 - "/img/2019-12-29_upgrade-python-on-macos_1.png"


aliases:
- "/upgrade-python-on-macos-b2e67aeece36"

---

The latest major version of Python 2 was originally release on 03/07/2010 and on 01/01/2020 Python 2 will be no more;

> We have decided that January 1, 2020, will be the day that we sunset Python 2. That means that we will not improve it anymore after that day, even if someone finds a security problem in it. You should upgrade to Python 3 as soon as you can.

Interestingly the latest version of macOS still ships with Python 2.7 as its default Python interpreter, however that won’t be for much longer as Apple are removing all Scripting Language Runtimes from future releases of macOS;

> Scripting language runtimes such as Python, Ruby, and Perl are included in macOS for compatibility with legacy software. Future versions of macOS won’t include scripting language runtimes by default, and might require you to install additional packages. If your software depends on scripting languages, it’s recommended that you bundle the runtime within the app.

So what’s the best way to upgrade to Python 3 on macOS? Having found myself stuck in dependancy hell before I decided do some reading and have settled on using [pyenv](https://www.mediaglasses.blog/p/152c96aa-4ff8-4827-ba21-da5abf725718/) which makes install and maintaining various versions of Python easy.

You can install it using [Homebrew](https://brew.sh/) with the following commands;

```
$ brew update $ brew install pyenv
```

Once installed, you simply need to install your desired version of Python, the current version at the time of writing is 3.8.0;

```
$ pyenv install 3.8.0 $ pyenv global 3.8.0 $ pyenv version
```

Now that the right version of newer version of Python is installed we can use the following command to make sure that the pyenv shims are correctly loaded each time you open a shell;

```
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\\n eval "$(pyenv init -)"\\nfi' >> ~/.zshrc
```

Once you have restarted your terminal session you should be able to run;

```
$ python --version
```

If everything has gone as planned you should see the following output;

![](/img/2019-12-29_upgrade-python-on-macos_1.png)
