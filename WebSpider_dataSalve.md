## 数据存储

在使用解析器解析出数据之后，需要将数据存储下来，可以存成简单的额TXT,JSON，CSV等，还可以保存到数据库中，如MySQL。

### 1.TXT文件存储

直接使用`open()`方法打开一个文件，`a`追加的形式写入文件即可。

### 2.JSON文件

JavaScript Object Notation，通过对象和数组的组合来表示数据。

#### 对象和数组

JS中，一切都是对象。任何支持的类型都可以中国JSON表示。

**JSON中的数据需要使用双引号，不能使用单引号**

- 对象：使用花括号{}包裹的内容，数据结构为{key1:val1,key2:val2,...}的键值对结构，key为对象的属性，val为对应的值。
- 数组：数组是用方括号[]包裹起来的内容，数据结构为["java","javascript","vb",....]的索引结构。

如一个JSON对象可以写成如下的形式:
```
[{
	"name":"Bob",
	"gender":"male"
	"birthday":"1992-10-18"
},{
	"name":"Selina",
	"gender":"female",
	"birthday":"1995-10-18"
}]
```
中括号相当于列表类型，列表中的元素可以是任意类型。

JSON可以由上两种形式自由组合而成，可以无限次嵌套，结构清晰。

#### 读取JSON文件

使用JSON库就可以实现对JSON文件的读写操作。可以调用JSON库中的`loads()`方法将JSON文本字符串转为JSON对象，通过`dumps()`方法将JSON对象转为文本字符转。

通过`loads()`方法能够将文本转换为Python中的列表和字典数据类型，然后就可以进行相应的索引和切片操作。

#### 输出JSON文件

调用`dumps()`方法可以将JSON对象转化为字符串。如果保存为JSON格式，需要加一个参数`indent`表示缩进的字符个数。这样会自动缩进，格式更加清晰。为了输出中文，可以指定`ensure_ascii`属性为`false`，这样能够显示中文。

例子：
```python
data = [{'name':'王伟'}]
with open('data.json' , 'w' , encoding = 'utf-8') as file:
    file.write(json.dumps(data, indent = 2 , ensure_ascii = False))
```

### 3.CSV文件存储

comma-Seprated Values，以纯文本形式存储表格数据。

### MySQL的存储

关系型数据库是给予关系模型的数据库，关系模型是通过二维表来存储的。存储方式是行列组成的表。每一列是一个字段，每一行是一条记录。表可以看做某个实体的集合，实体之间存在关系，需要表与表之间的关联关系来体现，多个表组成一个数据库，就是关系型数据库。

