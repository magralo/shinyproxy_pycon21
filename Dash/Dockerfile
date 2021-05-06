FROM python:3.7


# Dash and dependencies
RUN pip3 install dash==1.19.0 && \
    pip3 install pandas

RUN pip install flask
RUN pip install gunicorn

RUN mkdir apps
COPY apps/ /apps

EXPOSE 8050

WORKDIR /apps


#docker build . -t shinyproxy-demo-dash
### Coger el ID de la nueva imagen
#### docker tag 9ee36ef76544  magralo95/shinyproxy-demo-dash
#### docker push  magralo95/shinyproxy-demo-dash