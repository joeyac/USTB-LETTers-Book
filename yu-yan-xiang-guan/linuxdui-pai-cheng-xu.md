# Linux 对拍程序

## 使用方法：

新建 `match.sh`文件，复制程序代码；

添加执行权限`chmod +x match.sh`

如下方式调用

`./match.sh gen.cpp a-force.cpp a.cpp`

会默认生成数据到`data.in`文件，并且将输出根据暴力程序和需要对拍的程序的文件名生成对应的`.out`文件，对拍会一直进行直到找出错误为止，`ctrl+c`退出

## 代码：

```bash
if (($# < 3)); then
	echo three argument need.
	echo Sample:
	echo "./$(basename $0) gen.cpp a-force.cpp a.cpp"
	exit 0
fi
gen_name=`echo $1 | cut -d. -f1`
force_name=`echo $2 | cut -d. -f1`
cpp_name=`echo $3 | cut -d. -f1`

cnt=0
while true; do
	./$gen_name > data.out #出数据
	./$force_name < data.out > $force_name.out #正确（暴力）程序
	./$cpp_name < data.out > $cpp_name.out #被测程序
	let cnt+=1;
	if diff $force_name.out $cpp_name.out; then
		echo force:
		cat $force_name.out
		echo program:
		cat $cpp_name.out
		echo $cnt random match:AC
	else  
		echo $cnt random match:WA
		exit 0 
	fi
done

```



