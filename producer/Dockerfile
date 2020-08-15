FROM python:2.7-onbuild
ENV PYTHONPATH /usr/src/app
CMD [ 'python producer.py -p 5672 -s rabbitmq -m "Hello" -r 30' ]
