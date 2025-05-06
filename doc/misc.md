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

## mysql/mariadb data transferring between databases

### install mysql/mysqldump command line utilities

```shell
sudo apt install mysql-client-core
```

### backup/restore whole database (skipping some tables)

```shell
echo '==== setup ================================================'
srcHost=127.0.0.1
tarHost=127.0.0.1
srcDb="ecimp5-v2-template"
tarDb="ecimp5-30"
srcPort=3306
tarPort=3306
srcUser=root
tarUser=root
srcPass=pwd01
tarPass=pwd01
sqlFile=./backup-$(date +"%Y%m%d%H%M%S").sql
#echo '==== dump src db =========================================='
#export MYSQL_PWD="$srcPass"
#mysqldump -h"$srcHost" -P$srcPort -u"$srcUser" --column-statistics=0 "$srcDb" > "$sqlFile"
echo '==== dump src db (ignoring some tables) ==================='
export MYSQL_PWD="$srcPass"
argIgnoreTables=$(mysql -h"$srcHost" -P$srcPort -u"$srcUser" -D"$srcDb" -sNe "SHOW TABLES LIKE 'tbl_alarm_______';" | sed 's/^/ --ignore-table="'"$srcDb"'./g' | tr '\r\n' '"')$(mysql -h"$srcHost" -P$srcPort -u"$srcUser" -D"$srcDb" -sNe "SHOW TABLES LIKE 'tbl_monitor_______';" | sed 's/^/ --ignore-table="'"$srcDb"'./g' | tr '\r\n' '"')
echo $argIgnoreTables
cmd='mysqldump -h"'$srcHost'" -P'$srcPort' -u"'$srcUser'" --ignore-views '$argIgnoreTables' --column-statistics=0 "'$srcDb'" > "'$sqlFile'"'
echo $cmd
eval $cmd
echo '==== create and restore tar db ============================'
export MYSQL_PWD="$tarPass"
mysql -h"$tarHost" -P$tarPort -u"$tarUser" -sNe 'DROP DATABASE IF EXISTS `'"$tarDb"'`;'
mysql -h"$tarHost" -P$tarPort -u"$tarUser" -sNe 'CREATE DATABASE `'"$tarDb"'` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;'
mysql -h"$tarHost" -P$tarPort -u"$tarUser" --default-character-set=utf8mb4 --database="$tarDb" < "$sqlFile"
echo '==== remove file backup sql file =========================='
rm "$sqlFile"
```

### backup/restore with tables specified

```shell
echo '==== setup ================================================'
srcTables="tbl_variable tbl_data_source tbl_motion_equipment"
srcHost=127.0.0.1
tarHost=127.0.0.1
srcDb="ecimp5-v2-template"
tarDb="ecimp5-31"
srcPort=3306
tarPort=3306
srcUser=root
tarUser=root
srcPass=pwd01
tarPass=pwd01
sqlFile=./backup-$(date +"%Y%m%d%H%M%S").sql
echo '==== dump src db (tables specified) ======================='
export MYSQL_PWD="$srcPass"
mysqldump -h"$srcHost" -P$srcPort -u"$srcUser" --column-statistics=0 "$srcDb" $srcTables > "$sqlFile"
echo '==== create and restore tar db ============================'
export MYSQL_PWD="$tarPass"
#mysql -h"$tarHost" -P$tarPort -u"$tarUser" -sNe 'DROP DATABASE IF EXISTS `'"$tarDb"'`;'
#mysql -h"$tarHost" -P$tarPort -u"$tarUser" -sNe 'CREATE DATABASE `'"$tarDb"'` CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;'
mysql -h"$tarHost" -P$tarPort -u"$tarUser" --default-character-set=utf8mb4 --database="$tarDb" < "$sqlFile"
echo '==== remove file backup sql file =========================='
rm "$sqlFile"
```

## adding days on bash with date command

```shell
date '+%C%y%m%d' -d "$end_date+100 days"
```

