# consul



consul只有 server 模式才有备份



命令

```shell
./consul agent -server -bootstrap-expect=1 -ui -bind=127.0.0.1 -client=0.0.0.0 -data-dir=./data/ -config-dir=./config/
```

