FROM python:2.7
COPY requirements.txt /
ADD app_a.py /
ARG db_service_tag=db-service-internal
ENV db_service_tag=$db_service_tag
RUN sed -i "s/{{DB_SERVICE_NAME}}/${db_service_tag}/g" /app_a.py
RUN pip install -r ../requirements.txt
CMD [ "python", "./app_a.py" ]
