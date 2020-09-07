### container 連限至 本機的 localhost
從 container ，連限至 Mac 的 localhost，並不需要多做額外設定，
只需要在容器裡面，把目標 URL 改成 `http://host.docker.internal` ，即可。
參考[文件](https://docs.docker.com/docker-for-mac/networking/#use-cases-and-workarounds)


```bash
# in Mac machine
# in case I forget what this. It means build a quick server.
python -m SimpleHTTPServer 8000
```

```bash
# in container
docker run --rm -it alpine sh
apk add curl
curl http://host.docker.internal:8000
exit
```
