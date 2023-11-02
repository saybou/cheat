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

#### How to update Powerlevel10k

```bash
# Navigate to the Powerlevel10k directory
cd ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# Update from GitHub
git pull 
```

After updating, you may want to run `p10k configure` to see if there are new configuration options available.

#### Visual Studio Code Terminal Integration

I like to use the VS Code Terminal integration but initially the prompt provided by Powerlevel10k contained some broken characters. This is the custom font causing issues again, but its easy to fix.

- On Mac: press ⌘ , or click Code → Preferences → Settings.
- Enter terminal.integrated.fontFamily in the search box at the top of Settings tab and set the value below to `MesloLGS NF`.
- Restart VS Code and you are good.

### Enable useful plugins

Add following plugins in ```~/.zshrc```

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

You may have to install some missing plugins like :

- [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
- [zsh-nvm](https://github.com/lukechilds/zsh-nvm)
- [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

Install fzf (better search for command history)

[Install fzf with homebrew](https://github.com/junegunn/fzf#using-homebrew-or-linuxbrew)

## NVM (node version manager)

### Install

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

### Auto use

[https://github.com/nvm-sh/nvm#zsh](https://github.com/nvm-sh/nvm#zsh)

Calling `nvm use` automatically in a directory having a `.nvmrc` file

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

### Usage

[https://github.com/nvm-sh/nvm#usage](https://github.com/nvm-sh/nvm#usage)

To download, compile, and install the latest release of node :

```bash
nvm install node
```

## Git

### Config

#### Global config

```bash
# create a global gitignore file for all your git project, init with .DS_Store
echo .DS_Store >> ~/.gitignore_global

# add the file to your git config
git config --global core.excludesfile ~/.gitignore_global

# set your name
git config --global user.name "Your Name"

# set your email
git config --global user.email you@email.com

# activate color ui
git config --global color.ui true
```

#### Local config

```bash
cd [PATH_TO_REPOSITORY]
git config user.name "user name"
```

### Use VSCode as visual editor

Open VSCode > Cmd + Shift + P > Shell Command: Install 'code' command in PATH

```bash
git config --global -e
```

Add `editor = code --wait` in the `[core]` section.

### Reset a branch to a specific commit

- Local env

```bash
git reset --hard 00f7918
git commit --allow-empty -m "[NO-TICKET] Reverting to the state of the branch at 00f7918"
git push --force origin develop

# Then to prove it (it won't print any diff)
git diff develop..origin/develop
```

- If reset is not set on other env

```bash
git reset --hard origin/develop
```

### Push branch on origin

```bash
git push origin <mabranche>
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

### Edit specific commit (after feedback for example)

- interactive rebase

```bash
git rebase -i <commit_hash> # with alias : grbi 973487
```

set to "edit" (or "e") the commit you want to edit
Update the code and ammend the commit
Then continue rebase

```bash
git commit --ammend --no-edit # with alias : gcn!
git rebase --continue # with alias : grbc
```

- commit fixup

Edit your code then fixup specific commit

```bash
git commit --fixup 9ac993c9587343564fa7abb1a9402a95243a6fd9
```

Then you can rebase on a previous commit and squash the fixup

```bash
git rebase -i --autosquash <commit_hash> 
```

```bash
### Useful

```bash
git pull
git push origin develop
git commit -m "[DOC-0000] mon message"
git merge --no-ff mabranche -m "[DOC-0000] merge branche mabranche into develop"
```

[Git push options](https://docs.gitlab.com/ee/user/project/push_options.html)

## VSCode

Open File : `cmd + p`

Run command : `cmd + shift + p`

- `reload window`
- `restart TS server`

Recommended extensions :

- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
- [SonarLint](https://marketplace.visualstudio.com/items?itemName=SonarSource.sonarlint-vscode)
- [Path Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.path-intellisense)
- [Live Share](https://marketplace.visualstudio.com/items?itemName=MS-vsliveshare.vsliveshare)
- [Project Manager](https://marketplace.visualstudio.com/items?itemName=alefragnani.project-manager)
- [Markdown lint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint)
- [EditorConfig](https://marketplace.visualstudio.com/items?itemName=EditorConfig.EditorConfig)

## AWS

### Cloudwatch Insights

Filter logs

  ```text
  fields @timestamp, @message
  | sort @timestamp desc
  | filter @message like "my message"
  | limit 20
  ```

Check all logs associated to a request id

  ```text
  fields @timestamp, @message
  | sort @timestamp desc
  | filter @requestId like "BAwvMhZtjoEEP2w="
  | limit 20
  ```

### SSO example

```text
############ ~/.aws/config : SSO Login Configuration ############
[profile profile-name]
sso_start_url = https://xxx.awsapps.com/start
sso_region = eu-west-1
sso_account_id = xxx
sso_role_name = XxxRoleName
region = eu-west-1
output = json
```

```bash
# sso login
aws sso login --profile profile-name

# s3 copy example
aws s3 cp s3://my-super-bucket/folder/subfolder/ ./ --profile production-uno --recursive
```

## Useful bash commands

- User info

```bash
dscl . -list /Users UniqueID
id username
```

- What listen to port 80 ?

```bash
sudo lsof -i :80
```
