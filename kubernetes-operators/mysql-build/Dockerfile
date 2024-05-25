FROM python:3.10.14-alpine3.19
COPY templates ./templates
COPY mysql-operator.py ./mysql-operator.py 
RUN pip install kopf kubernetes pyyaml jinja2 
CMD kopf run /mysql-operator.py