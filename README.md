# Criando image docker segura

- Verify authenticity
- Signed files or valid checksums
- Use HTTPS
- Use GPG Keys

Best pratices

- FROM Directive:specific tag
- Never run as root
- User Directive: always
- Drop privileges ASAP
- gosu, not sudo

Minimal base images

- No cruft
- Alpine Linux: 2 MB
- Ubuntu: 60 MB

Exemples

```
FROM alpine:3.9

RUN apk add --update nginx && rm -fr /var/cache/apk/*

COPY index.html /usr/share/nginx/html/index.html
COPY nginx-nobody.conf /etc/nginx/nginx.conf

USER nobody

CMD ["nginx","-g","daemon off;"]

```
> Aprendizado: no caso só nginx, eu tive que alterar a porta padrão 80 para 8080, dele para que seja possível executar como `NOBODY`.

---

# Scan image

Esse scan, verifica as configurações do host, containers rodando e images existente no host, é verificado se os itens estão deacordo com as melhores praticas.

```
docker run -it --net host --pid host --userns host --cap-add audit_control \
    -e DOCKER_CONTENT_TRUST=$DOCKER_CONTENT_TRUST \
    -v /etc:/etc:ro \
    -v /usr/bin/docker-containerd:/usr/bin/docker-containerd:ro \
    -v /usr/bin/docker-runc:/usr/bin/docker-runc:ro \
    -v /usr/lib/systemd:/usr/lib/systemd:ro \
    -v /var/lib:/var/lib:ro \
    -v /var/run/docker.sock:/var/run/docker.sock:ro \
    --label docker_bench_security \
    docker/docker-bench-security
```


---

# Referencias
- [Nginx unprivileged](https://github.com/nginxinc/docker-nginx-unprivileged)
- [Best practices docker image](https://www.youtube.com/watch?v=LmUw2H6JgJo)
- [Docker bench security](https://github.com/docker/docker-bench-security)
- [Dockerfile best practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)
- [Intro guide to dockerfile bestpractices](https://www.docker.com/blog/intro-guide-to-dockerfile-best-practices/)
