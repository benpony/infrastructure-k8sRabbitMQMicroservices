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
    "-r", \
    "30" \
]

# FROM python:2.7-onbuild
# ENV PYTHONPATH /usr/src/app
# WORKDIR /app
# ADD producer.py /app
# ADD requirements.txt /app
# RUN pip install --requirement /app/requirements.txt
