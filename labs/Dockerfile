FROM alpine:3.9

RUN apk add --update nginx && rm -fr /var/cache/apk/* && export DOCKER_CONTENT_TRUST=1

COPY index.html /usr/share/nginx/html/index.html
COPY nginx-nobody.conf /etc/nginx/nginx.conf

USER nobody

CMD ["nginx","-g","daemon off;"]
