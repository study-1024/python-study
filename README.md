# python-study
notebook
# 用 doctest 做单元测试
!!!note  doctest
>在软件开发中包含对原始程序代码的自动的单元测试（unit testing）通常是最
佳作法。单元测试提供一种自动确认的方式，检查个别部份的程序代码，如函数，是否能正
确执行。这使得我们可以在后期变更一个函数的效果，并快速地测试它是否仍然可以完成它
该做的工作。

>Python 有一个内建的doctest 模块，可以做简单的单元测试。octest 可以写在三引号
字符串里面，放在函数主体或是脚本的第一行， 它们由直译器阶段范例组成，而这些范例包
含了一系列在 Python 提示符下的输入，并紧接着预期从 Python 直译器得到的输出。

>doctest 模块会自动执行任何由 >>> 开始的陈述，并且比对下一行程序代码与直译器所
输出的结果。
``````python
#要看这是如何运作的，将下列内容放在名为myfunctions.py 的脚本中：
def is_divisible_by_2_or_5(n):
	"""
		>>> is_divisible_by_2_or_5(8)
		True
	"""
if __name__ == '__main__':
	import doctest
	doctest.testmod()
#最后三行程序代码使doctest 得以执行，将它们放在任何包含 doctest 的档案底部。
#执行这个脚本会产生如下输出：
$ python myfunctions.py
**********************************************************************
File "myfunctions.py", line 3, in __main__.is_divisible_by_2_or_5
Failed example:
is_divisible_by_2_or_5(8)
Expected:
True
Got nothing
**********************************************************************
1 items had failures:
1 of 1 in __main__.is_divisible_by_2_or_5
***Test Failed*** 1 failures.
$
这是一个失败测试的例子，这个测试说：「如果你呼叫is_divisible_by_2_or_5(8) ，
结果应为True。」既然如上的is_divisible_by_2_or_5 并没有传回任何东西，测试就失败
了，并且 doctest 告诉我们它预期得到True 这个结果，但是没有得到任何东西。
我们可以直接传回 True 值以通过这个测试：
def is_divisible_by_2_or_5(n):
	"""
	>>> is_divisible_by_2_or_5(8)
	True
	"""
	return True
if __name__ == '__main__':
	import doctest
	doctest.testmod()

如果我们现在执行这个脚本并不会有任何输出，这表示这个测试通过了。再次注意
doctest 模块的字符串必须直接放在函数定义的标头之后，这样才能执行。
``````
``````python
#要看更多的输出细节，使用 -v 命令列选项呼叫这个脚本：
$ python myfunctions.py -v
Trying:
	is_divisible_by_2_or_5(8)
Expecting:
	True
ok
1 items had no tests:
	__main__
1 items passed all tests:
	1 tests in __main__.is_divisible_by_2_or_5
1 tests in 2 items.
1 passed and 0 failed.
Test passed.
$
#虽然测试通过了，我们的测试组却显然是不适当的，因为不论将什么自变量传入
#is_divisible_by_2_or_5 都会传回 True。这里有一个包含更完备的测试组和程序代码的完
整版本，可以通过这个测试：\
def is_divisible_by_2_or_5(n):
"""
>>> is_divisible_by_2_or_5(8)
True
>>> is_divisible_by_2_or_5(7)
False
>>> is_divisible_by_2_or_5(5)
True
>>> is_divisible_by_2_or_5(9)
False
"""
return n % 2 == 0 or n % 5 == 0
if __name__ == '__main__':
import doctest
doctest.testmod()
用-v 命令列选项执行这个脚本，然后看看你得到什么。
``````
