
### 1、字符串模式匹配

>[warning] Write a function called that takes a string of parentheses, and determines if the order of the parentheses is valid. The function should return `true` if the string is valid, and `false` if it's invalid.

**Examples:**

```js
"()"             	=>  true
")(()))"        	=>  false
"("              =>  false
"(())((()())())"  		=>  true
```

**Constraints:**

```js
0 <= input.length <= 100
```
>[warning] You may assume that the input string will only contain opening and closing parenthesis and will not be an empty string.

我的代码：

```js
//不好意思，又见递归
//递归将匹配到的()，替换掉，直到匹配不到，则false,为空则true
function validParentheses(parens){
  parens = parens.replace(/\(\)/g,'').trim();
  if(!parens){
    return true;
  }
  let match = parens.match(/\(\)/g);
  if(null != match && match.length>0){
    return validParentheses(parens);
  }else{
    return false;
  }
}
```

<div class="cline"></div>

```js
function validParentheses(parens){
  let tokenizer=/[()]/g,token,count=0;
  while(token = tokenizer.exec(parens),null!=token){
    if(token == '('){
      count++;
    }else if(token==')'){
      count--;
      if(count<0){
        return false;
      }
    }
  }
  return count == 0;
}
```

关于`exec`，记录一笔：

>[success] `RegExpObject.exec(string)`

[参考]（http://www.w3school.com.cn/jsref/jsref_exec_regexp.asp）

<div class="cline"></div>

```js
function validParentheses(parens){
  var indent = 0;
  
  for (var i = 0 ; i < parens.length && indent >= 0; i++) {
    indent += (parens[i] == '(') ? 1 : -1;    
  }
  
  return (indent == 0);
}
```

### 2、字符串替换

>[warning] Move the first letter of each word to the end of it, then add "ay" to the end of the word. Leave punctuation marks untouched.

**Examples:**

```js
pigIt('Pig latin is cool'); // igPay atinlay siay oolcay
pigIt('Hello world !');     // elloHay orldWay !
```


我的解法：

```js
function pigIt(str){
  return str.split(' ').map(item=>item.substring(1,item.length)+item.substr(0,1)+'ay').join(' ')
}
```
主要还是通过截取字符串，达到拼接的目的，这里有几点值得注意下：

* `substr`与`substring`的区别，前者返回一个从指定位置开始的指定长度的子字符串；后者是获取区间内的字符串。
* 字符串`item`可通过`item.charAt(0)`或者`item[0]`来获取首字母；
* 字符串的截取同样可以沿用数组的方法`slice`,跟字符串的截取方法差不多；

优化版本：
```js
function pigIt(str){
  return str.split(' ').map(item=>item.slice(1)+item[0]+'ay').join(' ')
}
```

<div class="cline"></div>

之前使用的字符串模板替换，在这里同样可用，沿用并巩固下：

```js
//简洁的很漂亮
function pigIt(str){
	return str.replace(/(\w)(\w+)/g,'$2$1ay')
}
```

### 3、

>[warning] Find the missing letter
Write a method that takes an array of consecutive (increasing) letters as input and that returns the missing letter in the array.
You will always get an valid array. And it will be always exactly one letter be missing. The length of the array will always be at least 2.
The array will always contain letters in only one case.

**Example:**

```js
['a','b','c','d','f'] -> 'e'
['O','Q','R','S'] -> 'P'
```

>[warning] (Use the English alphabet with 26 letters!)
Have fun coding it and please don't forget to vote and rank this kata! :-)
I have also created other katas. Take a look if you enjoyed this kata!

我的解法：

```js
function findMissingLetter(array)
{
	let result  = '';
	array.reduce((r,item)=>{
		let num = item.charCodeAt();
		if(r === 0){
			r = num;
		}
		if(Math.abs(num-r) >1){
			result = String.fromCharCode(((num+r)/2).toString());
		}
		return num;
		
	},0);
  
  return result;
}

```