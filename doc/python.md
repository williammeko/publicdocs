## Links
[[README](../README.md)]
[[python](<../doc/python.md>)]

# Python

## mirror setup

```shell
# list config
pip config list
pip config get global.index-url

# upgrade pip with mirror
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U

# config pip mirror
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
pip config set global.index-url https://mirrors.aliyun.com/pypi/simple/
```

```shell
# install some-package with temporary pip mirror
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
```
