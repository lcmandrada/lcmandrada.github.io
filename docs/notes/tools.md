# Tools

## Online

[Python REPL](https://www.programiz.com/python-programming/online-compiler/)  
[MongoDB Playground](https://mongoplayground.net/)  
[StackEdit: Markdown Editor](https://stackedit.io/app)  
[Sequence Diagram](https://sequencediagram.org/)  



## Library

[Pydantic Model Generator](https://docs.pydantic.dev/latest/integrations/datamodel_code_generator/p)



## WSL
Windows Subsystem for Linux

[Setup](https://docs.microsoft.com/en-us/windows/wsl/install)

1. Install WSL
    ```sh
    wsl -install
    ```
2. Reboot and finish setup
3. Set username and password
4. Update packages
    ```sh
    sudo apt update && sudo apt upgrade
    ```

**Tips**

- To open your WSL project in Windows File Explorer: `explorer.exe .`
- Store your project files on the same operating system as the tools you plan to use for the fastest performance speed.
- Run Linux tools from a Windows command line: `wsl ls -la`
- Run a Windows tool directly from the WSL command line: `notepad.exe .bashrc`
- Mix Linux and Windows commands: `wsl ls -la | findstr "git"` or `dir | wsl grep git`

**References**  
[Dev Environment](https://docs.microsoft.com/en-us/windows/dev-environment/) 
[Database](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database) 
[Docker](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-containers) 
[Basic Commands](https://docs.microsoft.com/en-us/windows/wsl/basic-commands) 
[Mount Disk](https://docs.microsoft.com/en-us/windows/wsl/wsl2-mount-disk) 
[GPU Acceleration](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gpu-compute) 
[GUI Apps](https://docs.microsoft.com/en-us/windows/wsl/tutorials/gui-apps) 



## Font
[Office Code Pro](https://www.fontsquirrel.com/fonts/office-code-pro) 



## Terminal

### Windows Terminal
[Setup](https://docs.microsoft.com/en-us/windows/terminal/install) 
[Settings](https://gist.github.com/lcmandrada/8152e4c6947ee0ef3d40b729e8087294) 


### Hyper.js
Electron-based terminal

[Setup](https://hyper.is/#installation) 
[Settings](https://gist.github.com/lcmandrada/725c4f56f4bb7f8bfe5dc8088f258dd7) 


### Oh My Zsh
Open source, community-driven framework for managing Zsh configuration

[Setup](https://ohmyz.sh/#install) 
[.zshrc](https://gist.github.com/lcmandrada/910990b940ef271e81992e25fcd33e1a)

```sh
sudo apt-get install zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# theme
ZSH_THEME="sammy"
```

### Fuzzy Finder
General-purpose command-line fuzzy finder

[Setup](https://github.com/junegunn/fzf#installation)
```sh
git clone -depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

### Snippets

#### Change and list directory
```sh
# ~/.zshrc
cd() { builtin cd "$@" && ls; }
```



## Theme

### Nord
[Ports](https://www.nordtheme.com/ports) 
[Firefox Theme](https://addons.mozilla.org/en-US/firefox/addon/nord-polar-night-theme/?utm_source=addons.mozilla.org&utm_medium=referral&utm_content=search) 
[Ubuntu](https://www.gnome-look.org/p/1267246/) 

```sh
sudo add-apt-repository universe
sudo apt install gnome-tweaks
```



## Visual Studio Code
Source-code editor

[Setup](https://code.visualstudio.com/download) 
Turn on Settings Sync

**References**  
[Unit Testing](https://code.visualstudio.com/docs/python/testing)



## Python

### pyenv
Switch between multiple versions of Python

[Build Dependencies](https://github.com/pyenv/pyenv/wiki#suggested-build-environment)
```sh
sudo apt-get update; sudo apt-get install make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm \
libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```

[Setup](https://github.com/pyenv/pyenv#installation)
```sh
# install
git clone https://github.com/pyenv/pyenv.git ~/.pyenv

# optional, for better performance
cd ~/.pyenv && src/configure && make -C src

# env variables
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zprofile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zprofile
echo 'eval "$(pyenv init -path)"' >> ~/.zprofile

# init
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
```

**Commands**
```sh
pyenv install -l
pyenv install 3.9.10
pyenv uninstall 3.9.10

pyenv version
pyenv versions

pyenv local
pyenv global
```

### Poetry
Python packaging and dependency management made easy

[Setup](https://python-poetry.org/docs/master/#installing-with-the-official-installer) 
Don't install with system Python, use a newly built Python with pyenv
```sh
# ~/.zshrc
export PATH="$HOME/.local/bin:$PATH"
```

**Tab Completion with Oh My Zsh**
```sh
mkdir $ZSH_CUSTOM/plugins/poetry
poetry completions zsh > $ZSH_CUSTOM/plugins/poetry/_poetry

# ~/.zshrc
plugins=(
    git
    poetry
)
```

**Commands**
```sh
poetry new project_name # creates a new directory
poetry init # creates poetry files inside a directory

poetry install
poetry show

poetry add package
poetry add -D package

poetry remove package
poetry remove -D package

poetry env list -full-path
poetry env remove env_name
```
