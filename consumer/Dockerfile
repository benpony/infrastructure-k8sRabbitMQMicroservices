FROM python:2.7-onbuild
ENV PYTHONPATH /usr/src/app
COPY consumer.py ${PYTHONPATH}
WORKDIR ${PYTHONPATH}
ENTRYPOINT [ "python", \
    "consumer.py", \
    "-p", \
    "5672", \
    "-s", \
    "rmq-rabbitmq.default.svc.cluster.local" \
]

# CMD [ 'python consumer.py -p 5672 -s rmq-rabbitmq.default.svc.cluster.local' ]
