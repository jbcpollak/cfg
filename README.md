Based on https://developer.atlassian.com/blog/2016/02/best-way-to-store-dotfiles-git-bare-repo/


# Updating

config pull
config submodule update --recursive --remote

# First Checkout of Repo On a New Computer

## Pre Requisites
On a Mac, ensure git is installed by running `xcode-select --install`
Ensure you are logged into git so the clone command below works correctly.

```
cd ~
REPO=https://github.com/jbcpollak/cfg
git clone --bare ${REPO}.git --recurse-submodules $HOME/.cfg
function config {
   /usr/bin/git --git-dir=$HOME/.cfg/ --work-tree=$HOME $@
}
mkdir -p .config-backup
config checkout
if [ $? = 0 ]; then
  echo "Checked out config.";
  else
    echo "Backing up pre-existing dot files.";
    config checkout 2>&1 | egrep "\s+\." | awk {'print $1'} | xargs -I{} mv {} .config-backup/{}
fi;
config checkout
config config status.showUntrackedFiles no
config submodule update --init --recursive
```


# Other MacOS Setup Scripts

* https://github.com/mathiasbynens/dotfiles/blob/master/.macos
* https://github.com/orrsella/dotfiles/blob/master/setup-macos.sh

