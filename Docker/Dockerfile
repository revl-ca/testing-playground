#
# Final image
#
FROM nginx:1.18.0@sha256:e4d21fc07f63bc115759f5826b04862053bd96b1a3bbfccb6b2a4cc92a2dc9cb

ENV OS_FAMILY="Debian GNU/Linux"
ENV OS_VERSION="10 (Buster)"

EXPOSE 80

ADD ./src/index.html /usr/share/nginx/html

