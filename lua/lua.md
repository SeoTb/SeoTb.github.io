### 申明变量
	local a = 1
	b = a
	nil: 为申明的变量

### 赋值
	a,b = 1,2
	print(a,b)

### 数值类型
	//支持16进制，科学计数法
	a = 0x11
	b = 2e10

### 字符串
	a = "abc\nded"
	//多行文本
	c = [[abc
	def
	ghijkl]]
	//字符串连接
	print(a..c)
	c = tostring(10)
	n = tonumber("123")
	//字符串长度
	print(#"abcdef") --6

```lua
s = string.char(0x30,0x00,0x31,0x32,0x33)
n = string.byte(s,2)
print(s)
print(n, #s)
```

### 函数
	function f(a,b)
		print(a,b)
	end
	f(1,2)

	//返回值支持多个
	function f1(a,b)
		return a-b,a+b
	end
	i,j = f1(3,2)
	print(i,j)

### table
	#数字下标，从1开始
	a = {1,"ab",function() end}
	#table插入值
	table.insert(a,"new")
	table.insert(a,2,"new1")
	#table删除元素 有返回值
	table.remove(a,2)

	#字符串下标
	a = {
		a = 1,
		b = 2,
		c = 3
	}
	print(a.a)
	print(a.b)

	#全局变量都在_G这个table中

### 真假
	print(1~=2) //true
	print(1>=2) //false

	# 与 或 非
	and or not

### 分支判断
	if 1 > 10 then
		print("a")
	elseif 1 <10 then
		print("b")
	else
		print("c")
	end

### 循环
	# 可以使用break, 没有continue
	for i = 10,1,2 do
		if i == 5 then break end
		print(i)
	end

	while n > 10 do
		if n == 12 then break end
		n = n - 1
		print(n)
	end