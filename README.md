# watch-and-run
监听目录中文件的变化然后执行, 类似于gulp, 但是这里只是提供一下命令
# fswatch安装
https://github.com/emcrisostomo/fswatch

# golang
```
fswatch -x -t -u . \
    | awk '{if( $0 ~ " IsFile" && $0 ~ " Updated" && $6 ~ ".go$") print "echo ["$4"] "$6";go run "$6; system("");}' \
    | sh
```
命令行执行此命令后, 自动监听当前目录下的文件， 如果.go结尾的文件发生修改, 就自动执行 go run 此文件
