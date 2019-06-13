# Mac Development Playbook

This playbook installs and configures most of the software I use on my Mac for web and software development. Some things in macOS are slightly difficult to automate, so I still have some manual installation steps, but at least it's all documented here.
#TODO: Consider forking gantsign/ansible_role_antigen

## Installation:
#### Command Line Tools
1. Download and extract the Apple Command Line Tools pkg from [here](https://developer.apple.com/download/more/).
2. Run the following two commands to install the package and then the headers
```bash
sudo installer -pkg \
  <path-to-command-line-tools>/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
sudo installer -pkg \
  /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```
#### Homebrew
```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
#### Python
1. Install pyenv and pipenv with brew
2. Add pyenv init to .bash_profile
3. Run `export PYTHON_CONFIGURE_OPTS="--enable-framework"` to ensure python compiles correctly for use with cmake
4. Install python 2 and 3 via pyenv
5. Set python version to 3.7.3
6. Upgrade pynvim (for use with YouCompleteMe vim plugin)
```bash
brew install pyenv pipenv
pyenv init
# Add eval "$(pyenv init -)" to .bash_profile
pyenv install 2.7.16
pyenv install 3.7.3
pyenv global 3.7.3
pip3 install --user --upgrade pynvim
pyenv rehash

```
#### Ansible
1. Install ansible with `brew install ansible`
3. Clone this repository to your local drive.
4. Run `$ ansible-galaxy install -r requirements.yml` inside this directory to install required Ansible roles.
5. Run `ansible-playbook main.yml -i inventory --check -K` inside this directory to do a test run. Enter your account password when prompted.
6. Run `ansible-playbook main.yml -i inventory -K` inside this directory to run the playbook for real. Enter your account password when prompted.

> Note: If some Homebrew commands fail, you might need to agree to Xcode's license or fix some other Brew issue. Run `brew doctor` to see if this is the case.

### Running a specific set of tagged tasks

You can filter which part of the provisioning process to run by specifying a set of tags using `ansible-playbook`'s `--tags` flag. The tags available are `dotfiles`, `homebrew`, `mas`, `extra-packages` and `osx`.

    ansible-playbook main.yml -i inventory -K --tags "dotfiles,homebrew"

## Overriding Defaults

Not everyone's development environment and preferred software configuration is the same.

You can override any of the defaults configured in `default.config.yml` by creating a `config.yml` file and setting the overrides in that file. For example, you can customize the installed packages and apps with something like:

    homebrew_installed_packages:
      - cowsay
      - git
      - go

Any variable can be overridden in `config.yml`; see the supporting roles' documentation for a complete list of available variables.

## Included Applications / Configuration (Default)

Applications (installed with Homebrew Cask):

  - [Docker](https://www.docker.com/)
  - [Firefox](https://www.mozilla.org/en-US/firefox/new/)
  - [Google Chrome](https://www.google.com/chrome/)
  - [Hyper](https://hyper.is)
  - [Visual Studio](https://visualstudio.microsoft.com)
  - [Visual Studio Code](https://code.visualstudio.com)
  - [.NET SDK](https://dotnet.microsoft.com)

Packages (installed with Homebrew):

  - git
  - hub
  - cmake
  - node
  - neovim
  
Dotfiles

- .zshrc
- .gitconfig
- .hyper.js
- .osx
- .neovim-init.vim

My [dotfiles](https://github.com/bginbey/dotfiles) are installed into ~/dotfiles/ with symbolic links in the home directory. The .neovim-init is also linked to Neovim's default location ./config/nvim/init.vim You can disable dotfiles management by setting `configure_dotfiles: no` in your configuration.

Finally, there are a few other preferences and settings added on for various apps and services.

## Future additions

### Things that still need to be done manually


### Applications/packages to be added:
  - [Figma](https://figma.com)

### Configuration to be added:


## Testing the Playbook

Many people have asked me if I often wipe my entire workstation and start from scratch just to test changes to the playbook. Nope! Instead, I posted instructions for how I build a [Mac OS X VirtualBox VM](https://github.com/geerlingguy/mac-osx-virtualbox-vm), on which I can continually run and re-run this playbook to test changes and make sure things work correctly.
