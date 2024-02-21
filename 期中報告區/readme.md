## 實作練習一:
- 已下載完成
- 啟動1:docker run -p 80:80 vulnerables/web-dvwa
- 啟動2:docker run -p 8080:80 vulnerables/web-dvwa
## 實作練習二: nginx 
- nginx docker
- https://hub.docker.com/_/nginx/
- 下載:docker pull nginx
- 啟動:docker run --name some-nginx -v /some/content:/usr/share/nginx/html:ro -d nginx

# 錯誤更正
```
Using default tag: latest
latest: Pulling from library/nginx
mediaType in manifest should be 'application/vnd.docker.distribution.manifest.v2+json' not 'application/vnd.oci.image.manifest.v1+json'
```

## 更新docker版本
- https://askubuntu.com/questions/472412/how-do-i-upgrade-docker
## docker version
```
docker version
Client: Docker Engine - Community
 Version:           19.03.0
 API version:       1.40
 Go version:        go1.12.5
 Git commit:        aeac9490dc
 Built:             Wed Jul 17 18:15:22 2019
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.0
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.5
  Git commit:       aeac9490dc
  Built:            Wed Jul 17 18:14:00 2019
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.6
  GitCommit:        894b81a4b802e4eb2a91d1ce216b8817763c29fb
 runc:
  Version:          1.0.0-rc8
  GitCommit:        425e105d5a03fabd737a126ad93d62a9eeede87f
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

```
