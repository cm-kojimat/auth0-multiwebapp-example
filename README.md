# auth0 で複数の webapp を試す

## 確認環境

```
❯ docker version
Client: Docker Engine - Community
 Cloud integration  0.1.18
 Version:           19.03.13
 API version:       1.40
 Go version:        go1.13.15
 Git commit:        4484c46d9d
 Built:             Wed Sep 16 16:58:31 2020
 OS/Arch:           darwin/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.13
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.13.15
  Git commit:       4484c46d9d
  Built:            Wed Sep 16 17:07:04 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          v1.3.7
  GitCommit:        8fba4e9a7d01810a393d5d25a3621dc101981175
 runc:
  Version:          1.0.0-rc10
  GitCommit:        dc9208a3303feef5b3839f4323d9beb36df0a9dd
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683

❯ docker-compose version
docker-compose version 1.27.4, build 40524192
docker-py version: 4.3.1
CPython version: 3.7.7
OpenSSL version: OpenSSL 1.1.1g  21 Apr 2020

❯ mkcert --version
v1.4.1

❯ node --version
v12.19.0
```

## 導入手順

### submodule の clone

```
❯ git submodule init

❯ git submodule update --recursive
```

### auth0 の import

```
cp config.json{.example,}
vim config.json
npm ci
npx a0deploy import -c config.json -i auth0/tenant.yaml
```

### cert の設定

```
mkdir -p etc/certs/
mkcert -install
mkcert -cert-file etc/certs/shared.crt -key-file etc/certs/shared.key "*.local.gd"
```

### Application の設定

```
mkdir -p etc/app{1,2,3}
vim etc/app{1,2}/auth_config.json
vim etc/app3/.env
```

### docker-compose の起動

```
docker-compose up --detach --build
open https://app1.local.gd
```
