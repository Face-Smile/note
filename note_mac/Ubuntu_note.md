#### Ubuntu 忽略软件包更新

```shell
apt-mark hold <package-name>
```

或者：

```shell
echo "package-name hold" | dpkg --set-selections
```

或者：

```shell
aptitude unhold package_name
```





查看软件包状态：

```shell
dpkg --get-selections
```

