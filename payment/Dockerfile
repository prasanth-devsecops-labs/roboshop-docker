FROM python:3.9.25-alpine3.22 AS builder
WORKDIR /app
RUN apk add --no-cache build-base linux-headers pcre-dev
COPY requirements.txt .
RUN pip3.9 install --prefix=/install -r requirements.txt

FROM python:3.9.25-alpine3.22
EXPOSE 8080
WORKDIR /app
RUN apk add --no-cache pcre
RUN addgroup -S roboshop && adduser -S roboshop -G roboshop
RUN chown -R roboshop:roboshop /app
COPY --from=builder /install /usr/local
COPY --chown=roboshop:roboshop payment.ini *.py requirements.txt ./
USER roboshop
CMD ["uwsgi", "--ini", "payment.ini"]


# FROM almalinux:9
# RUN dnf install -y python3 python3-devel gcc && dnf clean all
# WORKDIR /app
# COPY payment.ini payment.py rabbitmq.py requirements.txt /app/
# RUN pip3 install -r requirements.txt
# # ENV CART_HOST="cart" \
# #     CART_PORT="8080" \
# #     USER_HOST="user" \
# #     USER_PORT="8080" \
# #     AMQP_HOST="rabbitmq" \
# #     AMQP_USER="roboshop" \
# #     AMQP_PASS="roboshop123"
# CMD ["uwsgi", "--ini", "payment.ini"]
