#!/bin/bash
echo "Hello world !"

# bash

shell变量 

    - 定义：变量名与等号之间不能有空格
            定义时变量名之前不加$号

    - 使用定义过的变量，在变量名前加$使用，可使用花括号标识${变量边界}
      已被定义的变量可以被重新定义(可以理解为 赋值)

    - 只读变量 readonly
      删除变量 unset

    - 变量类型：局部变量 、环境变量、shell变量

shell字符串

    - 单引号、双引号与PHP类似，不同的是单引号中完全不会转义

    - 字符串操作： 拼接无需操作符
                  获取字符串长度 string="abcd"
                                echo ${#string}
                  提取字符串操作 echo ${string:2:2} # 输出 cd
                  查找子字符串   echo `expr index "$string" ac`
                                # 查找a或c的位置 输出 1

shell数组

    - 数组定义： 采用空格或者换行分割元素
                array_name=(value0 value1)
                array_name_copy=(
                value0
                value1
                )
                或单独定义
                array_name[0]=value
                下标范围没有限制，可以不连续

    - 读取数组： ${数组名[下标]}
                使用@可以获取数组的所有元素
                ${数组名[@]}

    - 获取数组长度： 获取个数 ${#array_name[@]}
                    获取元素长度 ${#array_name[k]}

shell注释

    - 单行注释： #
    - 多行注释： :<<EOF          :<<'             :<<!
                注释内容         注释内容          注释内容
                EOF             '                !
EOF
shell传参

    - 获取参数： 获取单个参数 $n n代表一个数字 $0 代表执行的文件名
                获取多个参数 $* "$*"以单字符串输出 "$1 $2 $3"
                            $@ "$@"以多字符串输出 "$1" "$2" "$3"
    
    - 特殊参数： $# 获取传递到脚本参数个数
                $$ 脚本执行的当前进程ID号
                $! 后台运行的最后一个进程ID号
                $- 显示shell当使用前选项 与set命令相同
                $? 显示最后命令的退出状态 0为没有错误

shell运算符

    - 算数运算符：shell的运算通过其它命令实现，如awk和expr(apple原生支持)
      注意： 表达式与运算符之间有空格，且被``包含
        - 加法： + `expr $a + $b`
          减法： - `expr $a - $b`
          乘法： * `expr $a \* $b`
          除法： / `expr $a / $b`
          取余： % `expr $a % $b`
          赋值： = a=$b
          相等： == [ $a == $b ]    # 比较数字
          不相等： != [ $a != $b ]  # 比较数字

    - 另外的运算方式
        - 条件表达式 $result=$[value1 + value2]
        - apple方式 $result=$(($value1 + $value2)) 此处乘法*不需要转义

    - 关系运算符：只支持数字，除非字符串的值是数字
        - 相等：   -eq [ $a -eq $b ]
          不相等： -ne [ $a -ne $b ]
          大于：   -gt [ $a -gt $b ]
          小于：   -lt [ $a -lt $b ]
          大于等于： -ge [ $a -ge $b ]
          小于等于： -le [ $a -le $b ]

    - 布尔运算符：布尔运算(true 与 false 的运算)
        - 非运算： ! [ ! false ]
          或运算： -o [ $a -lt 20 -o $b -gt 100 ]
          与运算： -a [ $a -lt 20 -a $b -gt 100 ]

    - 逻辑运算符
        - 逻辑AND： && [[ $a -lt 100 && $b -gt 100 ]]
          逻辑OR:   || [[ $a -lt 100 || $b -gt 100 ]]

    - 字符串运算符
        - 相等：         = [ $a = $b ]
          不相等：       != [ $a != $b ]
          长度是否为0：   -z [ -z $a ]
          长度是否不为0： -n [-n "$a" ]
          是否不为空：       [ $a ]

    - 文件测试运算符
        - 块设备文件：         -b [ -b $file ]
          字符设备文件：       -c [ -c $file ]
          目录：              -d [ -d $file ]
          普通文件：           -f [ -f $file ]
          SGID位：            -g [ -g $file]
          黏着位(Sticky Bit)： -k [ -k $file ]
          有名管道：            -p [ -p $file ]
          SUID位：             -u [ -u $file ]
          可读：               -r [ -r $file ]
          可写：               -w [ -w $file ]
          可执行：             -x [ -x $file ]
          不为空(文件大小)：    -s [ -s $file ]
          存在：               -e [ -e $file ]

shell echo命令

    - 显示字符： 双引号 引用变量、转义符、文本格式符(如：\n \t)
                单引号 屏蔽所有引用
                无引号 引用变量、转义符
      开启转义(用于文本格式符) -e echo -e "OK!\n"    # -n 换行
                                echo -e "Hello!\c" # -c 不换行

    - 结果定向： echo "string" > file

    - 显示执行结果： echo `命令`

    - read命令： 通过read命令从标准输入中读取一行，并把每个字段赋给shell变量
                read a b
                echo "第一个参数：$a； 第二个参数：$b"
        - read参数
            -p 输入提示文字
            -n 输入字符长度限制
            -t 输入限时
            -s 隐藏输入内容
        - 注意 read命令读入词组按空格进行分隔，如果词组数量大于需要参数个数
               多出的词组将作为整体被最后一个参数接收

shell printf命令

    - printf命令由POSIX标准定义，因此移植性比echo好
        printf format-string [arguments...]
    - 字符串遵循shell规则，其中"\c"只在%b参数中生效

shell test 命令

    - 检查某个条件是否成立，可以对数值、字符、文件进行测试
        实际上类似条件判断 test -e ./nofile -o -e hello.c

shell 流程控制

    -区别：流程控制主体不可为空

    - if语句： if condition1
               then
                   command1
               elif condition2
               then
                   conmand2
               else
                   conmand3
               fi

    - for循环： 指令部分变量名不用加$美元符号
        列表格式 for var in item1 item2 ... itemN
                do
                    command
                done
        c格式 for ((expr1;expr2;expr3))
              do
                  command
              done
        for循环还可以读取:
            范围       for num in {1..10}
            命令行输出  for username in `awk -F:'{print $1}' /etc/passwd`
            传入参数    for params

    - while语句：  while condition
                   do
                       command
                   done
        无限循环 'while ：' or 'while true' or 'for (( ; ; ))'

    - until语句：  until condition
                  do
                      command
                  done

    - case语句： 一旦模式匹配，执行完匹配模式后不再继续其他模式
        结构 case $value in
             1)
                 command1
                 ;;
             2|3)
                 command2
                 ;;
             *)
                 command*
                 ;;
             esac

    - continue 与 break： 与c语言类似

shell 函数

    - 定义格式  func_name(){
                    command
                }
    
    - 返回值 最后一行的命令运行结果将返回 成为外部变量
            retrun 数值 将返回数值0~255，通过$?即可读取
                (没有指定return时，$?为0表示没有错误)
        示例：
            addDemo(){
                result=$(($1 + $2))
            }
            echo result # 内容为$1+$2
            echo $?     # 内容为0

    - 参数 通过$n 命令读取， ${10}表示第十个参数 "$10"表示"${1}0"

shell 输入输出重定向

    - 输出重定向 command > file      echo "hello" > users
        追加    command >> file

    - 输入重定向 command < file      wc -l < users
        追加    command << file
file
    - 重定向与文件
        标准输入文件->stdin  文件描述符->0
        标准输出文件->stdout 文件描述符->1
        标准错误文件->stderr 文件描述符->2

        将输出文件n、m合并  command > file n>&m
        将输入文件n、m合并  command > file n<&m

    - Here Document
        command << delimeter
            document
delimeter
        将两个dilimeter之间的内容作为输入传入给command
        注意：结尾的delimeter一定要顶格写，前后不能有任何字符

    - /dev/null 特殊文件，可以达到屏蔽输出的作用  $command > /dev/null 2>&1

shell 包含文件

    - 包含标识符 .           . ./file_name  #注意两个点之间有空格
                source      source ./file_name
        注意：被包含的脚本文件不需要执行权限
