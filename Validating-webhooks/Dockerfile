FROM python:3.9-alpine
WORKDIR /app
COPY requirements.txt /app/requirements.txt
RUN pip3 install  -r /app/requirements.txt
COPY validating_webhook.py /app
COPY webhook.crt /app
EXPOSE 5000
COPY webhook.key /app
CMD ["python", "validating_webhook.py"]
