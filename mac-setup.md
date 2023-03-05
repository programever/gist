**Remove mouse acceleration**

defaults write .GlobalPreferences com.apple.mouse.scaling -1

**Make symbolic link**

ln -s ~/Workspace/dotfiles/.zshrc ~/.zshrc

ln -s ~/Workspace/dotfiles/.tmux.conf ~/.tmux.conf

ln -s ~/Workspace/dotfiles/.gemrc ~/.gemrc

mkdir .config

ln -s ~/Workspace/dotfiles/tmuxinator ~/.config/tmuxinator

mkdir .config/nvim

ln -s ~/Workspace/dotfiles/coc-settings.json ~/.config/nvim/coc-settings.json

ln -s ~/Workspace/dotfiles/init.vim ~/.config/nvim/init.vim


**Extract ssh in dropbox**

mv ~/Dropbox/Documents/Migrating/.ssh ~/.ssh

**Add ipv6 to /etc/hosts**

127.0.0.1 localhost

255.255.255.255 broadcasthost

::1 ip6-localhost


**Install git from Xcode Terminal command line**

git config --global user.name Iker

git config --global user.email tdtrinh.web@gmail.com


**Brew**

/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

brew services list


**Install fira font**

brew tap homebrew/cask-fonts

brew install --cask font-fira-code


**FNM**

brew install fnm


**ELM**

npm install -g elm create-elm-app elm-test @elm-tooling/elm-language-server elm-format elm-oracle elm-review

**Haskell**

Install ghcup ONLY, do not install the rest from https://www.haskell.org/ghcup/

cabal install ormolu hlint

Use `ghcup tui` to install the rest


**PureScript**

npm install -g purescript spago purescript-language-server purs-tidy

brew install dhall-lsp-server


**Bash**

brew install bash shfmt shellcheck

npm i -g bash-language-server


**VIM**

https://github.com/elm/compiler/tree/master/installers/mac

brew install reattach-to-user-namespace tmuxinator tmux neovim

npm install -g neovim

sudo gem install neovim

curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

brew install the\_silver\_searcher

brew install python (AFTER THAT RESTART)

pip3 install neovim

pip3 install --upgrade neovim

:PlugInstall in NVIM

Add Shift+Return to Send Hex Code: 0x1B


**GRADLE**

brew install gradle


**Firebase**

npm install -g firebase-admin firebase-tools firebase-functions

firebase login

firebase projects:list


**Apache**

sudo launchctl unload -w /System/Library/LaunchDaemons/org.apache.httpd.plist 2\>/dev/null

brew install httpd

brew services start httpd

Config path /usr/local/etc/httpd/httpd.conf


**PHP**

brew install php

MAY NEED TO RUN brew reinstall openssl


**MySQL**

brew install mysql

brew services start mysql

Setup root user use password 123123

mysql\_secure\_installation


**RVM**

echo "gem: --no-document" \>\> ~/.gemrc

curl -L https://get.rvm.io | bash -s stable --auto-dotfiles --autolibs=enable --rails

source /Users/iker/.rvm/scripts/rvm

RESTART TERMINAL

