#!/bin/bash
echo "Hello world !"

# bash

shell变量 

	- 定义：变量名与等号之间不能有空格
			定义时变量名之前不加$号

	- 使用定义过的变量，在变量名前加$，可使用花括号标识${变量边界}
	  已被定义的变量可以被重新定义(可以理解为 赋值)

	- 只读变量 readonly
	  删除变量 unset

	- 变量类型：局部变量 、环境变量、shell变量

shell字符串

	- 单引号、双引号与PHP类似，不同的是单引号中完全不会转义

	- 字符串操作： 拼接无需操作符
				  获取字符串操作 string="abcd"
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
				下表范围没有限制，可以不连续

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
