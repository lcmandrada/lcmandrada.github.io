# Celery

`celery.py`
```py
from celery import Celery
from settings import BROKER_URL, REDIS_MASTER

app = Celery('tasks')
app.conf.broker_url = BROKER_URL
app.conf.broker_transport_options = {'master_name': REDIS_MASTER}
```

`send.py`
```py
from celery import app

# task queue
app.send_task(
    name='engine.tasks.test_data',
    task_id=7,
    queue='engine',
    kwargs={
        ...
    }
)

# task listener
@app.task(acks_late=True)
def test_task(...):
    ...
```

`cli`
```bash
$ celery -A path.to.tasks worker -l info -Q queue_name # path in this case is `send`
```
