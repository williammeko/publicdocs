## Links
[[README](../../README.md)]
[[docker gitlab](<../docker-gitlab/README.md>)]

# docker gitlab

## docker host folder

```shell
mkdir ~/Archives/gitlab-20260520
```

## docker-compose.yml

>> [docker-compose.yml](<./docker-compose.yml>)

## run gitlab up

```shell
cd ~/Archives/gitlab-20260520
docker-compose up -d
```

## gitlab website 

### get initial password for root user

```shell
# likely
docker exec -it gitlab-20260520-web-1 grep 'Password:' /etc/gitlab/initial_root_password
```

### login/register

http://wmbp02.local:7080/

- root
    - for gitlab setup/admin
    - password from [get initial password for root user](<#get-initial-password-for-root-user>)
- other users
    - for access gitlab with git commands
    - registered by each git user

## backup docker image to file

```shell
# commit to new image (likel be gitlab-20260520-web-1)
docker commit gitlab-20260520-web-1 gitlab:202605210940
# save to file also compress
docker save gitlab:202605210940 | gzip > ./gitlab-202605210940.tar.gz
# restore docker image from file
docker load -i ./gitlab-202605210940.tar.gz
```
