# Commands

## Git

### Rename local branch
```bash
git branch -m new-branch-name
```

## Kubectl

### Logs
```bash
k logs -f deploy/<deploy>
k logs -fl app=<label>
```

### Logs of previous deploy
```bash
kubectl logs podname -c containername --previous

k logs deploy/<deploy> --previous
```

### Execute into a pod
```bash
k get pods
k exec -ti <pod> -- sh
```

### Run cronjob as job
```bash
k create job --from=cj/<cronjob> <name>
```

### Get pods and sort by memory
```bash
k top pod --sort-by=memory
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