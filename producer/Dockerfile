FROM python:2.7-onbuild
ENV PYTHONPATH /usr/src/app
COPY producer.py ${PYTHONPATH}
WORKDIR ${PYTHONPATH}
ENTRYPOINT [ "python", \
    "producer.py", \
    "-p", \
    "5672", \
    "-s", \
    "rmq-rabbitmq.default.svc.cluster.local", \
    "-m", \
    "'Hello'", \
    "-r","30" \
]

# CMD [ 'python producer.py -p 5672 -s rabbitmq -m "Hello" -r 30' ]
