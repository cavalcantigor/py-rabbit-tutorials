# Python - RabbitMQ Tutorials

A series of tutorials using RabbitMQ with python and `pika`. These tutorials assume RabbitMQ is installed an running on `localhost` on standard port `5672`.

To install RabbitMQ using docker click [here](https://hub.docker.com/_/rabbitmq). The file `requirements.txt` has all python dependencies you need to run all python files.

``` pip install -r requirements.txt ```

## Hello world!
First tutorial with simple producer and consumer scripts.

First you need to run consumer:

```shell
python receive.py
```

Then, you run producer to send *"Hello World"* message:

``` shell
python send.py
```

> https://www.rabbitmq.com/tutorials/tutorial-one-python.html

## Work Queues
This tutorial creates a *Work Queue* that will be used to distributed time-consuming tasks among multiple workers.

You need three consoles open. Two will run the `worker.py` script. These consoles will be our two consumers.

```
# shell 1
python worker.py
```

```
# shell 2
python worker.py
```

In the third one we'll pushing new tasks. Once you've started the consumers, you can publish a few messages.

```
python new_task.py First message.
python new_task.py Second message..
python new_task.py Third message...
python new_task.py Fourth message....
python new_task.py Fifth message.....
```

> https://www.rabbitmq.com/tutorials/tutorial-two-python.html

## Publish/Subscribe
In this tutorial we'll deliver a message to multiple consumers. This pattern is known as "publish/subscribe".

You can spawn logs with
```
python emit_log.py [log_level]
```

And receive messages with
```
python receive_logs.py [log_level]
```

You can test differents receivers with differentes log levels and spawn multiple messages on each log level to test the pub/sub pattern.

> https://www.rabbitmq.com/tutorials/tutorial-three-python.html

## Routing
In this tutorial we're going to add a feature to it - we're going to make it possible to subscribe only to a subset of the messages. For example, we will be able to direct only critical error messages to the log file (to save disk space), while still being able to print all of the log messages on the console.

If you'd like to see all the log messages on your screen, open a new terminal and do:

```
python receive_logs_direct.py info warning error
```

And, for example, to emit an error log message just type:

```
python emit_log_direct.py error "Run. Run. Or it will explode."
```

> https://www.rabbitmq.com/tutorials/tutorial-four-python.html

## Topics
In our logging system we might want to subscribe to not only logs based on severity, but also based on the source which emitted the log. You might know this concept from the syslog unix tool, which routes logs based on both severity (info/warn/crit...) and facility (auth/cron/kern...).

To receive all the logs run:

```
python receive_logs_topic.py "#"
```

To receive all logs from the facility "kern":

```
python receive_logs_topic.py "kern.*"
```

Or if you want to hear only about "critical" logs:

```
python receive_logs_topic.py "*.critical"
```

You can create multiple bindings:

```
python receive_logs_topic.py "kern.*" "*.critical"
```

And to emit a log with a routing key "kern.critical" type:

```
python emit_log_topic.py "kern.critical" "A critical kernel error"
```

> https://www.rabbitmq.com/tutorials/tutorial-five-python.html