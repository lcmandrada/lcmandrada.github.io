# Commands

## Git

### Config
```sh
# set identity
git config -global user.name "Your Name"
git config -global user.email "youremail@domain.com"
```

### Initialize
```sh
# initialize directory as git project
git init

# repository is aliased by origin
git clone <repository> <local-directory>
```


### Update
```sh
# downloads commits, files, refs from remote to local
# safer update, fetched content has to be explicitly checked out
git fetch

# fetch and update local content
git pull <repository> <branch>

# merge giver to receiver, must be at receiver branch
git merge <giver-branch>
```

### Workflow
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

### Show
```sh
# show commits
git log

# HEAD commit contains the latest commits
git show HEAD

# show repositories
git remote -v
```

### Branch
```sh
# show current branch
git branch

# switch branch
git checkout <branch>

# delete branch
git branch -d <branch>

# rename local branch
git branch -m new-branch-name
```

### Reset
```sh
# revert file in working directory to its latest commit
git checkout <commit> <file>

# unstage a file
git reset <commit> <file>

# undo to the commit identified by sha
git reset <sha>
```



## Kafka

### Consume
```bash
kafka-console-consumer --topic invoice_sync --from-beginning --bootstrap-server localhost:9092

kafka-consumer-groups --bootstrap-server localhost:9092 --group payment_update_group --reset-offsets --shift-by 1 --topic payment_update --execute
```

### Produce
```bash
kafka-console-producer --topic payment_update --bootstrap-server localhost:9092
```



## Python

### Run Uvicorn
```bash
poetry run uvicorn main:app --reload --port 5001
```



## Shell

### Add multiple SSH keys
```bash
eval "$(ssh-agent -s)"
ssh-add -K ~/.ssh/id_rsa
```

### Export `.env` manually
```bash
export $(grep -v '^#' .env | xargs)
unset $(grep -v '^#' .env | sed -E 's/(.)=./\1/' | xargs)
```

### Tunnel from server
```bash
# accessible at: localhost:4000, localhost:4001, localhost:4002
ssh -NL 4000:localhost:4000 -NL 4001:localhost:4001 -NL 4002:localhost:4002 uat@server

# tunnel an external resource from a server
ssh -NL 5433:x.rds.amazonaws.com:5432 uat@server
```



## Slack

### GitHub
```bash
/github subscribe UploanPH/athena-company pulls reviews comments
/github unsubscribe UploanPH/athena-company issues commits releases deployments
```