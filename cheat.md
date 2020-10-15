# Cheat Sheet

## Package manager

[Install Homebrew](https://docs.brew.sh/Installation)

## iTerm + zsh + Oh My Zsh

[iTerm2](https://www.iterm2.com/) \
[Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh/wiki) \
[Powerlevel10k](https://github.com/romkatv/powerlevel10k#oh-my-zsh)

### Install zsh

```bash
brew install zsh

# check where is zsh
which zsh # /usr/local/bin/zsh
# set zsh as your default shell
chsh -s /usr/local/bin/zsh
```

### Install ohmyzsh

```install ohmyzsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

### Install theme Powerlevel10k

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

Set ```ZSH_THEME="powerlevel10k/powerlevel10k"``` in ```~/.zshrc```.

Open new terminal :)

### Enable useful plugins

Set plugin in ```~/.zshrc```

```bash
plugins=(
  git
  zsh-autosuggestions
  virtualenv
  history-substring-search
  z
  docker
  docker-compose
  colored-man-pages
  zsh-nvm
  zsh-syntax-highlighting
)
```

You will need to install missing plugins like :

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-nvm](https://github.com/lukechilds/zsh-nvm)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

Install fzf (better search for command history)

[Install fzf with homebrew](https://github.com/junegunn/fzf#using-homebrew-or-linuxbrew)

## NVM (node version manager)

[https://github.com/nvm-sh/nvm](https://github.com/nvm-sh/nvm)

Installing with curl

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.36.0/install.sh | bash
```

Check that you have the following lines in your `~/.zshrc`

```bash
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

--

[https://github.com/nvm-sh/nvm#zsh](https://github.com/nvm-sh/nvm#zsh)

Calling nvm use automatically in a directory with a .nvmrc file

Add the following lines in your `~/.zshrc`

```bash
# place this after nvm initialization!
autoload -U add-zsh-hook
load-nvmrc() {
  local node_version="$(nvm version)"
  local nvmrc_path="$(nvm_find_nvmrc)"

  if [ -n "$nvmrc_path" ]; then
    local nvmrc_node_version=$(nvm version "$(cat "${nvmrc_path}")")

    if [ "$nvmrc_node_version" = "N/A" ]; then
      nvm install
    elif [ "$nvmrc_node_version" != "$node_version" ]; then
      nvm use
    fi
  elif [ "$node_version" != "$(nvm version default)" ]; then
    echo "Reverting to nvm default version"
    nvm use default
  fi
}
add-zsh-hook chpwd load-nvmrc
load-nvmrc
```

--

[https://github.com/nvm-sh/nvm#usage](https://github.com/nvm-sh/nvm#usage)

To download, compile, and install the latest release of node :

```bash
nvm install node
```

___

## Command

```bash
dscl . -list /Users UniqueID
id username
```

## Web

What listen to port 80 ?

```bash
sudo lsof -i :80
```

## Git

### config

```bash
echo .DS_Store >> ~/.gitignore_global

git config --global core.excludesfile ~/.gitignore_global

git config --global user.name "Your Name"

git config --global user.email you@email.com

git config --global color.ui true

# Local repository config
cd [PATH_TO_REPOSITORY]
git config user.name "user name"
```

### Reset Git to a specific commit

- En local :

```bash
git reset --hard 00f7918
git commit --allow-empty -m "[NO-TICKET] Reverting to the state of the branch at 00f7918"
git push --force origin develop

# Then to prove it (it won't print any diff)
git diff develop..origin/develop
```

- Sur test, si le reset n'est pas pris en compte :

```bash
git reset --hard origin/develop
```

### Push branch on origin

```bash
git push origin mabranche
```

### Remove branch from remote and locally

```bash
git push origin --delete <branch_name>
git branch -d <branch_name>
```

### Remove stale branches

```bash
git remote prune origin
```

### Special removed :D

```bash
# It provides you with a list of merged branches.
git branch --merged

# Here you indicate what branches should not be removed.
git egrep -v "(^\*|master|dev)"

# All the branches that remain after the egrep command are passed as an argument one by one to $ git branch -d (--delete) to be removed.
git xargs git branch -d
```

### Pull in the changes your colleague pushed to the remote repository and rebase your commits on top to form a proper update structure

```bash
git pull --rebase --autostash
```

### Useful

```bash
git pull
git push origin develop
git commit -m "[DOC-0000] mon message"
git merge --no-ff mabranche -m "[DOC-0000] merge branche mabranche into develop"
```

## AWS

[AWS login Unify (with Okta)](https://login.unifygroup.com/app/UserHome)
