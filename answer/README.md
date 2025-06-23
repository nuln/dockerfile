# answer-docker

Docker image for [answer](https://answer.apache.org).

## build

```bash
docker build --build-arg VERSION=1.5.1 -t answer .
```

## usage

```bash
docker run -d -p 9080:80 -v answer-data:/data --name answer apache/answer
```
