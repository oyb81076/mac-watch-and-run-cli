# watch-and-run
监听目录中文件的变化然后执行, 类似于gulp, 但是这里只是提供一下命令
# fswatch安装
https://github.com/emcrisostomo/fswatch
```brew install fswatch```
# golang
```
fswatch -x -t -u . \
    | awk '{if( $0 ~ " IsFile" && $0 ~ " Updated" && $6 ~ ".go$") print "echo ["$4"] "$6";go run "$6; system("");}' \
    | sh
```
命令行执行此命令后, 自动监听当前目录下的文件， 如果.go结尾的文件发生修改, 就自动执行 go run 此文件
# clang
```
#!/usr/bin/env bash
SRC=`echo $PWD/src/ | awk '{print length($0) + 1}'`
mkdir -p build;
fswatch -x -t -u . \
  | awk '{if( $0 ~ " IsFile" && $0 ~ " Updated" && $6 ~ ".c$") print $6; system("")}' \
  | awk '{print substr($0, '$SRC', length($0) - '$SRC' - 1); system("")}' \
  | awk '{print "echo ["$0"] && gcc src/"$0".c -o build/"$0".o && ./build/"$0".o"; system("")}' \
  | sh
```
运行脚本后, 自动编译src目录下的文件到build中国然后运行
