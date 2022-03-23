## Links
[[README](../README.md)]
[[mqtt](<../doc/mqtt.md>)]
[[mqtt performance test](<../doc/mqtt performance test.md>)]

# MQTT

## docker-mqtt

```shell
mkdir /Users/will/mosquitto

docker run -it -p 1883:1883 -p 9001:9001 -v mosquitto.conf:/Users/will/mosquitto/config/mosquitto.conf -v /Users/will/mosquitto/data -v /Users/will/mosquitto/log eclipse-mosquitto
```

## MQTT CLI

> [https://www.hivemq.com/blog/mqtt-cli/#:~:text=The%20MQTT%20CLI%20is%20an,operations%20(PUBLISH%20and%20SUBSCRIBE).](https://www.hivemq.com/blog/mqtt-cli/#:~:text=The%20MQTT%20CLI%20is%20an,operations%20(PUBLISH%20and%20SUBSCRIBE).)

### publisher/sender

```shell
mqtt pub ...
```

### subscriber/receiver

```shell
mqtt sub -h 192.168.1.155 -p 1883 -i subClient01 -t practiceTopic01 -V 3 --debug
mqtt sub -h localhost -p 1883 -i subClient01 -t practiceTopic01 -V 3 --debug
```

```shell
mqtt sub -h localhost -p 1883 -i subClient01 -t topic01 -V 3 > /mnt/d/Files/topic01.txt
mqtt sub -h localhost -p 1883 -i subClient02 -t topic02 -V 3 > /mnt/d/Files/topic02.txt
```
