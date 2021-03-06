正则表达式：1、匹配字符；2、匹配位置

`/ab{2,5}c/` 表示匹配这样一个字符串：第一个字符是“a”，接下来是2到5个字符“b”，最后是字符“c”  

`/ab{2,5}c/g` 后面多了g，它是正则的一个修饰符。表示全局匹配，即在目标字符串中按顺序找到满足匹配模式的所有子串，强调的是“所有”，而不只是“第一个”。  

`/a[123]b/` 可以匹配如下三种字符串："a1b"、"a2b"、"a3b"。  


#### 范围表示法  
`[123456abcdefGHIJKLM]` 可以写成`[1-6a-fG-M]`。用连字符-来省略和简写。  
要匹配“a”、“-”、“z”这三者中任意一个字符,不能写成[a-z]，因为其表示小写字符中的任何一个字符,可以写成如下的方式：[-az]或[az-]或[a\-z]。即要么放在开头，要么放在结尾，要么转义。  

#### 排除字符组  
`[^abc]` 表示是一个除"a"、"b"、"c"之外的任意一个字符。字符组的第一位放^（脱字符）, 表示求反的概念。  


>`\d` 就是`[0-9]`。表示是一位数字。记忆方式：其英文是digit（数字）。  
`\D` 就是`[^0-9]`。表示除数字外的任意字符。  
`\w` 就是`[0-9a-zA-Z_]`。表示数字、大小写字母和下划线。记忆方式：w是word的简写，也称单词字符。  
`\W` 是`[^0-9a-zA-Z_]`。非单词字符。  
`\s` 是`[ \t\v\n\r\f]`。表示空白符，包括空格、水平制表符、垂直制表符、换行符、回车符、换页符。记忆方式：s是space character的首字母。  
`\S` 是`[^ \t\v\n\r\f]`。 非空白符。  
`.` 就是`[^\n\r\u2028\u2029]`。通配符，表示几乎任意字符。换行符、回车符、行分隔符和段分隔符除外。记忆方式：想想省略号...中的每个点，都可以理解成占位符，表示任何类似的东西。  

如果要匹配任意字符怎么办？可以使用`[\d\D]、[\w\W]、[\s\S]和[^]`中任何的一个。

#### 量词
>`{m,}` 表示至少出现m次。  
`{m}` 等价于{m,m}，表示出现m次。  
`?` 等价于{0,1}，表示出现或者不出现。记忆方式：问号的意思表示，有吗？  
`+` 等价于{1,}，表示出现至少一次。记忆方式：加号是追加的意思，得先有一个，然后才考虑追加。  
`*` 等价于{0,}，表示出现任意次，有可能不出现。记忆方式：看看天上的星星，可能一颗没有，可能零散有几颗，可能数也数不过来。  


##### 贪婪匹配和惰性匹配

```JavaScript
//贪婪匹配
var regex = /\d{2,5}/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); 
// => ["123", "1234", "12345", "12345"]
```
其中正则/\d{2,5}/，表示数字连续出现2到5次。会匹配2位、3位、4位、5位连续数字。
```JavaScript
//惰性匹配
var regex = /\d{2,5}?/g;
var string = "123 1234 12345 123456";
console.log( string.match(regex) ); 
// => ["12", "12", "34", "12", "34", "12", "34", "56"]
```
其中/\d{2,5}?/表示，虽然2到5次都行，当2个就够的时候，就不在往下尝试了。  

通过在量词后面加个问号就能实现惰性匹配，因此所有惰性匹配情形如下：
> `{m,n}?`  
`{m,}?`  
`??`  
`+?`  
`*?`  


#### 多选分支  
(p1|p2|p3)，其中p1、p2和p3是子模式，用|（管道符）分隔，表示其中任何之一。  

有个事实我们应该注意，比如我用`/good|goodbye/`，去匹配"goodbye"字符串时，结果是"good"。  
```JavaScript
var regex = /good|goodbye/g;
var string = "goodbye";
console.log( string.match(regex) ); 
// => ["good"]
```
而把正则改成/goodbye|good/，结果是：
```JavaScript
var regex = /goodbye|good/g;
var string = "goodbye";
console.log( string.match(regex) ); 
// => ["goodbye"]
```
也就是说，分支结构也是惰性的，即当前面的匹配上了，后面的就不再尝试了。 











