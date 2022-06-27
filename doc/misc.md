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


