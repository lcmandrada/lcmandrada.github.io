# Tools

## Online

[Python REPL](https://www.programiz.com/python-programming/online-compiler/)  
[MongoDB Playground](https://mongoplayground.net/)  
[StackEdit: Markdown Editor](https://stackedit.io/app)  
[Sequence Diagram](https://sequencediagram.org/)  



## Library

[Pydantic Model Generator](https://docs.pydantic.dev/latest/integrations/datamodel_code_generator/p)



## Git

**Config**
```sh
# set identity
git config -global user.name "Your Name"
git config -global user.email "youremail@domain.com"
```

**Initialize**
```sh
# initialize directory as git project
git init

# repository is aliased by origin
git clone <repository> <local-directory>
```

[//]: # (more)

**Update**
```sh
# downloads commits, files, refs from remote to local
# safer update, fetched content has to be explicitly checked out
git fetch

# fetch and update local content
git pull <repository> <branch>

# merge giver to receiver, must be at receiver branch
git merge <giver-branch>
```

**Workflow**
```sh
# show status
git status
git diff <. | file>

# add files to staging
git add <. | files>

# commit staged files
# must be in quotation marks
# written in present tense
# should be brief, 50 characters or fewer
git commit -m <comment>

# push committed files
git push origin <branch>
```

**Show**
```sh
# show commits
git log

# HEAD commit contains the latest commits
git show HEAD

# show repositories
git remote -v
```

**Branch**
```sh
# show current branch
git branch

# switch branch
git checkout <branch>

# delete branch
git branch -d <branch>
```

**Reset**
```sh
# revert file in working directory to its latest commit
git checkout <commit> <file>

# unstage a file
git reset <commit> <file>

# undo to the commit identified by sha
git reset <sha>
```



## SQL
Structured Query Language

**Relational Database** - a database that organizes information into one or more tables  
**Table** - a collection of data organized into rows and columns  
**Column** - a set of data values of a particular type  
**Row** - a single record in a table

**Statement**
- text that the database recognizes as a valid command
- always end in a semi-colon

**Clauses**
- perform specific tasks in SQL
- by convention, clauses are written in capital letters
- can also be referred to as commands

**Parameter** - a list of columns, data types, or values that are passed to a clause as an argument.

**Constraints**
```sql
PRIMARY KEY
UNIQUE
NOT NULL
DEFAULT
```

**CRUD**
```sql
- create table
CREATE TABLE label (
    label datatype constraints,
    label datatype constraints,
    ...
);

- insert
INSERT INTO table_name (column_names) VALUES (values);

- update
UPDATE table_name
SET column_name = value
WHERE column_name operator value;

ALTER TABLE table_name ADD COLUMN column_name datatype;

- delete
DELETE FROM table_name
WHERE column_name operator value;
```

**Select**
```sql
- select
SELECT * FROM table_name;
SELECT column_name, column_name FROM table_name;
SELECT column_name AS 'alias' FROM table_name;

- distinct
SELECT DISTINCT column_name FROM table_name;

- where
SELECT column_name
FROM table_name
WHERE column_name operator value;

SELECT column_name FROM table_name WHERE column_name LIKE pattern;
SELECT column_name FROM table_name WHERE column_name BETWEEN value AND value;
SELECT column_name FROM table_name ORDER BY column_name ASC|DESC;
SELECT column_name FROM table_name LIMIT value;

- case
SELECT column_name
    CASE
        WHEN column_name operator value THEN value;
        ...
        ELSE value
    END AS value
FROM table_name;

- aggregates
SELECT COUNT(column_name) FROM table_name;
SELECT SUM(column_name) FROM table_name;
SELECT MIN(column_name) FROM table_name;
SELECT MAX(column_name) FROM table_name;
SELECT AVG(column_name) FROM table_name;
SELECT ROUND(column_name, decimal_places) FROM table_name;

- group
SELECT column_name
FROM table_name
GROUP BY <col-label or number>
HAVING column_name operator value;

- join
SELECT column_name
FROM table1 [LEFT]
JOIN table2
    ON table1.column_name operator table2.column_name;

- union
SELECT column_name
FROM table1
UNION
    SELECT column_name FROM table1;

- intermediate
WITH label AS (
    ...
)
SELECT column_name
FROM label
JOIN table2
    ON condition;
```

**Clause**
```sql
clause table_name (
    parameter
);
```

**Operators**
```sql
- comparison operators
=
!=
>
<
>=
<=

- logical operators
AND
OR

- pattern
*       everything
_       exactly one character
%       zero or more characters
```

`IS [NOT] NULL` is a condition in SQL that returns true when the value is NULL and false otherwise.



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



## Docker
[Setup](https://docs.docker.com/desktop/windows/wsl/) 
[Best Practices](https://testdriven.io/blog/docker-best-practices) 

[Dockerfile with Poetry](https://github.com/python-poetry/poetry/issues/1178) 
- no need for a requirements file
- the virtualenv is managed by Poetry
- no Poetry in the final image
- application and venv contained in one folder
- Python application cannot write to its files or the virtualenv

[Dockerfile Templates](https://www.mktr.ai/the-data-scientists-quick-guide-to-dockerfiles-with-examples/) 

**Commands**
```sh
docker build . -t tag:latest

docker images
docker run -it image_id

# overrides CMD
docker run image_id uvicorn config.asgi
# overrides ENTRYPOINT
docker run -entrypoint uvicorn config.asgi image_id

# run a terminal inside container
docker run -it -entrypoint /bin/sh image_id

docker ps
docker ps -a

docker stop container_id
docker kill container_id

docker rm container_id
docker rmi image_id

# clean resources
docker image prune
docker rm $(docker ps -a -f status=exited -q)
docker volume prune

# dangling only
docker system prune
# includes unused
docker system prune -a
```



## MongoDB
[Setup](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database#install-mongodb) 
[Compass](https://www.mongodb.com/try/download/compass) 

MongoDB Shell  
Also a JS shell - can execute JS code

```js
help

db
show dbs
show collections

// create/switch to specified db
use [db]

// insert many documents
db.[collection].insert([{ ... }]) // array of js objects

// returns all documents; returns a cursor
db.[collection].find()
// specific query; returns a cursor
db.[collection].find({ ... })
// specific query; returns 0 or 1 document
db.[collection].findOne({ ... })

// updates 0 or 1 document
db.[collection].updateOne({ <query> }, { <operator> : { <updates> } })
// updates many documents
db.[collection].updateMany({ <query> }, { <operator> : { <updates> } })
// completely replace a document except the id
db.[collection].replaceOne({ <query> }, { <operator> : { <updates> } })

// deletes 0 or 1 document
db.[collection].deleteOne({ <query> })
// deletes many documents
db.[collection].deleteMany({ <query> })

// sample query operators
{'nested.properties': value}
{age: {$gt: 7}}
```
