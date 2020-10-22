FROM python:3.7
ENV PYTHONPATH /usr/src/app/
WORKDIR ${PYTHONPATH}
COPY producer.py requirements.txt ${PYTHONPATH}
RUN pip install -r requirements.txt
ENTRYPOINT [ "python", \
    "producer.py", \
    "-p", \
    "5672", \
    "-s", \
    "rabbitmq.default.svc.cluster.local", \
    "-m", \
    "'Hello'", \
    "-r", \
    "30" \
]
