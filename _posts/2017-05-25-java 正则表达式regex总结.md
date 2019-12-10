---
layout:     post
title:      java 正则表达式regex总结
subtitle:    java 正则表达式regex总结
date:       2017-05-25
author:     漆黑的夜漫天的星
header-img: img/home-bg-o.jpg
catalog: true
tags:
    - java基础
---

正则表达式用的不是太多，但总是经常忘记，所以总结一下，下次忘了一看马上就可以回想起来。

### 一、示例
```
package test;

public class Test4 {
	public static void main(String[] args) {
		String str = "abcdefghjk";
		String str2 = "lmnopqrst";
		// 匹配是否以abc开头
		String regx1 = "^abc+.*$";
		System.out.println(str.matches(regx1));// true
		System.err.println(str2.matches(regx1));// false
		// 匹配字符中是包含mn
		String regx2 = "^.*mn.*$";
		System.out.println(str.matches(regx2));// false
		System.out.println(str2.matches(regx2));// true
		// 匹配是否以rst结尾
		String regx3 = "^.*rst+$";
		System.out.println(str.matches(regx3));// false
		System.out.println(str2.matches(regx3));// true
	}
}
```


### 二、常用正则表达式

规则 | 正则表达式语法 
---|---
一个或多个汉字                          |\^[\u0391-\uFFE5]+$ 
邮政编码                                |\^[1-9]\d{5}$
QQ号码                                  |\^[1-9]\d{4,10}$ 
邮箱                                    |\^[a-zA-Z_]{1,}[0-9]{0,}@(([a-zA-z0-9]-*){1,}\.){1,3}[a-zA-z\-]{1,}$ 
用户名（字母开头 + 数字/字母/下划线）   |\^[A-Za-z][A-Za-z1-9_-]+$
手机号码                                |\^1[3|4|5|8][0-9]\d{8}$ 
URL                                     |\ ^((http|https)://)?([\w-]+\.)+[\w-]+(/[\w-./?%&=]*)?$ 
18位身份证号                            |\^(\d{6})(18|19|20)?(\d{2})([01]\d)([0123]\d)(\d{3})(\d|X|x)?$

 


### 三、正则表达式语法 

\^  表示匹配字符串的开始位置  (例外  用在中括号中[ ] 时,可以理解为取反,表示不匹配括号中字符串)  
\$  表示匹配字符串的结束位置  
\*  表示匹配 零次到多次  
\+  表示匹配 一次到多次 (至少有一次)  
\?  表示匹配零次或一次  
\.  表示匹配单个字符   
\|  表示为或者,两项中取一项  
\(  ) 小括号表示匹配括号中全部字符  
\[  ] 中括号表示匹配括号中一个字符 范围描述 如[0-9 a-z A-Z]  
\{  } 大括号用于限定匹配次数  如 {n}表示匹配n个字符  {n,}表示至少匹配n个字符  {n,m}表示至少n,最多m  
\\  转义字符 如上基本符号匹配都需要转义字符   如 \*  表示匹配*号  
\\w 表示英文字母和数字  \W  非字母和数字  
\\d  表示数字   \D  非数字  