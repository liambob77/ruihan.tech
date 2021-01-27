---
title: 'Python distribute Celery'
date: '2021/1/22'
authors: 'Liam bob'
tags: python celery
---

## celery env setup

Install and config Redis

```bash
sudo apt-get install redis
cp /etc/redis/redis.conf

# modify following line
> bind 127.0.0.1 ::1
> protected-mode yes
## into

> # bind 127.0.0.1 ::1
> protected-mode no

# start redis with custom config
sudo redis-server /etc/redis/redis_all.conf &

```

install python modules

```python
pip install redis
pip install celery
```

## simple celery example

```python
# task.py
from celery import Celery
from time import sleep

REDIS_IP = x.x.x.x

app = Celery('tasks', backend=REDIS_IP, broker=REDIS_IP)

@app.task
def async_job(x, y):
    sleep(0.1)
    return x + y
```

Install `celery` and `redis`.
Start worker on each machine.

```bash
celery -A tasks worker -l INFO
```

Stop worker on each machine, `Ctrl + c`

Start worker on each machine in the background.

```bash
celery multi start w1 -A tasks -l INFO
```

Restart worker on each machine in the background.

```bash
celery multi restart w1 -A tasks -l INFO
```

Stop worker on each machine in the background.

```bash
celery multi stop w1 -A tasks -l INFO
```

On client machine, with ipython as example

```python
from tasks import cadd
%timeit -n 1 -r 1 [cadd.delay(i, i) for i in range(1000)]
```

You will found task is running on each hosts.

## celery Manual routing

Assign different queque for each tasks

```python
from kombu import Exchange, Queue
app.conf.task_queues = {
    Queue('feed_tasks',    routing_key='feed.#'),
    Queue('regular_tasks', routing_key='task.#'),
    Queue('image_tasks',   exchange=Exchange('mediatasks', type='direct'),
                           routing_key='image.compress'),
}

app.conf.task_routes = {
    ('feed.tasks.*', {'queue': 'feeds'}),
    ('web.tasks.*', {'queue': 'web'}),
    (re.compile(r'(video|image)\.tasks\..*'), {'queue': 'media'}),
}

```

Note: celery 4.0 introduced new lower case settings and setting organization.
      for example: `CELERY_QUEUES` is old style config, which is replace by `task_queues`.