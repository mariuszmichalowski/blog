---
title: "Dotfiles"
author: "Russ Mckendrick"
date: 2014-08-10T11:00:00.000Z
lastmod: 2021-07-31T12:31:49+01:00

tags:
 - Tech
 - Github
 - Mac

cover:
    image: "/img/2014-08-10_dotfiles_0.png" 
images:
 - "/img/2014-08-10_dotfiles_0.png"
 - "/img/2014-08-10_dotfiles_1.png"


aliases:
- "/dotfiles-4b896a743118"

---

For the last few years I have been grabbing some we [documented dotfiles from GitHub](https://github.com/search?o=desc&q=dotfiles&ref=cmdform&s=stars&type=Repositories) but most of them have either ended up reconfigured my Mac to the point of it being unrecognisable or they have just been a collection of useful aliases. The I came across [Bashstrap](https://github.com/barryclark/bashstrap), it was close enough to what I wanted so I [forked it](https://github.com/russmckendrick/dotfiles) ….

![screenshot_otyb2f](/img/2014-08-10_dotfiles_1.png)

You can install them using the following commands;

```
git clone git@github.com:russmckendrick/dotfiles.git ~/.dotfiles
sudo easy_install Pygments
brew install tree
mv ~/.bash_profile ~/.dotfiles/backups/
mv ~/.bashrc ~/.dotfiles/backups/
mv ~/.gitconfig ~/.dotfiles/backups/
ln -s ~/.dotfiles/.bash_profile ~/.bash_profile
ln -s ~/.dotfiles/.bashrc ~/.bashrc
ln -s ~/.dotfiles/.gitconfig ~/.gitconfig
ln -s ~/.dotfiles/.hushlogin ~/.hushlogin
ln -s ~/.dotfiles/z.sh ~/.z.sh
```

Once installed you can do stuff like;

- s . or s filename.txt will open your current directory or a file in [Sublime Text 2](http://www.sublimetext.com/2)
- m README.md will open the specified file in [Marked 2](http://marked2app.com/)
- Jump directories rapidly, without having to set aliases using [Z](https://github.com/rupa/z)
- Syntax highlighted ‘cat’

amongst other things;

[Dotfiles Example](https://asciinema.org/a/11378 "https://asciinema.org/a/11378")

