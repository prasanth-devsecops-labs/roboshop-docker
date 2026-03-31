FROM almalinux:9
RUN dnf install -y python3 python3-devel gcc && dnf clean all
WORKDIR /app
COPY payment.ini payment.py rabbitmq.py requirements.txt /app/
RUN pip3 install -r requirements.txt
# ENV CART_HOST="cart" \
#     CART_PORT="8080" \
#     USER_HOST="user" \
#     USER_PORT="8080" \
#     AMQP_HOST="rabbitmq" \
#     AMQP_USER="roboshop" \
#     AMQP_PASS="roboshop123"
CMD ["uwsgi", "--ini", "payment.ini"]
