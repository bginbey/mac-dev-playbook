# Mac Development Playbook

This playbook installs and configures most of the software I use on my Mac for web and software development. There are still a few manual steps, documented here.

## Install before running the Playbook

#### Command Line Tools
1. Download and extract the Apple Command Line Tools from [here](https://developer.apple.com/download/more/).
2. Extract and run the pkg installer then run the following commands to install the package and then the headers
```bash
sudo installer -pkg \
  /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

#### Homebrew
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

#### Python via brew
recommended for now due to [common build problems](https://github.com/pyenv/pyenv/wiki/common-build-problems) with pyenv.
```bash
export PYTHON_CONFIGURE_OPTS="--enable-framework"
brew install readline xz python
pip3 install --user --upgrade pynvim
```

#### Python via pyenv
Not recommended for now due to [common build problems in OS X 10.14 Mojave and other platforms](https://github.com/pyenv/pyenv/wiki/common-build-problems<Paste>).
1. Install pyenv and pipenv with brew
2. Add pyenv init to .bash_profile
3. Run `export PYTHON_CONFIGURE_OPTS="--enable-framework"` to ensure python compiles correctly for use with cmake
4. Install dependencies with `brew install readline xz`
4. Install python 2 and 3 via pyenv
5. Set python version to 3.7.3
6. Upgrade pynvim (for use with YouCompleteMe vim plugin)
```bash
brew install pyenv pipenv
pyenv init
# Add eval "$(pyenv init -)" to .bash_profile
export PYTHON_CONFIGURE_OPTS="--enable-framework"
brew install readline xz
pyenv install 2.7.16
pyenv install 3.7.3
pyenv global 3.7.3
pyenv rehash
pip3 install --user --upgrade pynvim
```

#### Ansible
1. Install ansible via brew
3. Clone this repository
4. Install required roles
5. Run the playbook in check mode
6. Run the playbook
7. Running individual tasks by tag
```bash
# Install ansible
brew install ansible

# Clone this repository
git clone https://github.com/bginbey/mac-dev-playbook.git

# Install roles (from repo directory)
ansible-galaxy install -r requirements.yml

# Run playbook in check mode
# Sequentially dependent tasks may appear to fail
ansible-playbook main.yml -i inventory --check -K -vvvv 

# Run playbook
ansible-playbook main.yml -i inventory -K -vvvv

# Run tasks by tag
ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"
```

## Overriding Defaults
Any variable can be overridden in `config.yml`. For example, you can customize the installed packages and apps with something like:

```yml
homebrew_installed_packages:
  - cowsay
  - git
  - go
```

## Included applications and configuration

### Brew casks
  - [Docker](https://www.docker.com/)
  - [.NET SDK](https://dotnet.microsoft.com)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Google Drive File Stream](https://www.google.com/drive/download/)
  - [Hyper](https://hyper.is)
  - [KeypassX](https://www.keepassx.org)
  - [Private Internet Access](https://www.privateinternetaccess.com/)
  - [ShiftIt](https://github.com/fikovnik/ShiftIt)
  - [Visual Studio Code](https://code.visualstudio.com)
  - [Visual Studio](https://visualstudio.microsoft.com)

### Brew taps
  - git
  - hub
  - cmake
  - node
  - neovim
  
### Dotfiles
[Dotfiles](https://github.com/bginbey/dotfiles) are installed into ~/dotfiles/ with symbolic links in the home directory. The .neovim-init is also linked to Neovim's default location ./config/nvim/init.vim You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.
- .zshrc
- .gitconfig
- .hyper.js
- .osx
- .antigen
- .neovim-init.vim

### Zsh and Antigen bundles
- git
- heroku
- pip
- lein
- command-not-found

## Future development
- [Figma](https://figma.com)


## Testing the Playbook
See instructions for how to build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which playbooks may be run for testing.
