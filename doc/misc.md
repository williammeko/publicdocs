## Links
[[README](../README.md)]
[[misc](<../doc/misc.md>)]

# misc

## cnpm

> https://npmmirror.com/

> https://github.com/cnpm/cnpm

### install

```
npm install cnpm -g --registry=https://registry.npmmirror.com
```

### Sync packages from npm

```
cnpm sync [moduleName]
```

### Open package document or git web url

```
cnpm doc [name]
cnpm doc -g [name] # open git web url directly
```

### Build your own private registry npm cli

```
$ npm install cnpm -g

# then alias it
$ alias mynpm='cnpm --registry=https://registry.npm.example.com \
  --registryweb=https://npm.example.com \
  --userconfig=$HOME/.mynpmrc'
```

### Install with original npm cli

```
cnpm i --by=npm react-native
```

## docusaurus

> https://docusaurus.io/

```
npx create-docusaurus@latest docs-docusaurus classic
cd docs-docusaurus
npx docusaurus start --port 3030 --host 0.0.0.0

mv path/to/publicdocs ./docs/publicdocs
mv path/to/privatedocs ./docs/privatedocs
```

## ifconfig

```shell
sudo ifconfig en0 alias 192.168.0.200/24 up
sudo ifconfig en0 -alias 192.168.0.200
```

## port forwarding/nat on windows
```
netsh interface portproxy add v4tov4 listenport=3390 listenaddress=0.0.0.0 connectport=3389 connectaddress=192.168.0.103
netsh interface portproxy show all
netsh interface portproxy delete v4tov4 listenport=3390 listenaddress=0.0.0.0
```

## port forwarding/nat on osx/macos
```
ssh -L 0.0.0.0:28080:172.16.2.58:28080 -N 127.0.0.1
```

## file management - minio
```
alias minioserver='podman run -p 9000:9000 -p 9001:9001 -e "MINIO_ROOT_USER=hui" -e "MINIO_ROOT_PASSWORD=huihuihui" minio/minio server /Users/will/podman/data --console-address ":9001"'
```
