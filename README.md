# -DOM
在jquery操作下，必须知道如何优化dom  

···
文档对象模型 (DOM) 是HTML和XML文档的编程接口
javascript 分为 ECMAscript 另一部分就是 DOM，操作HTML文档，在浏览器中 DOM 和 javascript独立实现的。dom分离允许其他语言操作domVBScript。
https://developer.mozilla.org/zh-CN/docs/Web/API/Document_Object_Model/Introduction


书上的 比喻很好：js访问DOM好比中间有座桥，每次过桥都要交过路费，次数越多，费用就越高。

DOM访问以及修改

不要遍历修改修改DOM，用局部的变量存储修改中的内容

innerHTML与DOM方法比较：在低版本的浏览器innerHTML比原生的DOM方法效率高，但是webkit内核的浏览器用DOM是略胜一筹。
innerHTML添加DOM可以通过 数组（array）push字符串的方法操作。最后再用array.join("")来拼接字符串。

节点克隆
上面原生的创建DOM，再次提高效率可以用cloneNode
var tr = document.createElement('tr'); var nextTr = tr.cloneNode(true) true代表深层拷贝，false代表浅层拷贝


HTML集合  类数组列表  length 和 index获取对应的值
注意：：：HTML集合一直与文档保持着联系，每次需要最新的信息，他都会重复执行查询过程，这也是低效率的原因。
var divLen =document.getElementsByTagName('div');
 for(var  i = 0;i<divLen.length;i++){
       document.body.appendChild( document.createElement('div') )
 }
这是一个死循环，因为div在不停的添加；length没有终止。

dom循环要比arr数组要慢的多，因为每次都要重新查询。
可以通过把html集合收集到一个arr中，在进行循环,也就是吧html集合进行缓存，遍历缓存的数组。
但是也要注意：这里显示搜集HTML集合，在进行一次遍历，所以也带来了消耗。具体情况酌情考虑。
function toArray(coll){
  for(var i=0,a=[],len=coll.length;i<len;i++){
    a[i]=coll[i]
  }
  return a;
}

html方法
document.getELementsByName() document.getElementsByClassName() document.getElementsByTagName()
html属性
document.images;
document.links
document.forms
document.forms[0].elements 第一个表单所有的字段


访问集合元素时使用局部变量，而不是直接访问具体的查找的路径的元素*******
先缓存length；然后可以缓存collection，最后缓存要多次访问的元素*********
function clooectionLoacl(){
  var coll = document.getElementsByTagName("div"),len=coll.length,name='';
  for(var count =0;count<len;count++){
    //这里访问的coll
    name=coll[count].nodeName;
    name=coll[count].nodeType
    name=coll[count].tagName
  }
}

最快的方法
function clooectionLoacl(){
  var coll = document.getElementsByTagName("div"),len=coll.length,name=''，ele=null;
  for(var count =0;count<len;count++){
    //这里访问的coll
    ele=count[i]
    name=ele.nodeName;
    name=ele.nodeType
    name=ele.tagName
  }
}

获取DOM元素  选择效率更高的寻找DOM的api方法  元素节点与其他类型节点，我们是查找元素，所以最好用区分元素节点的API
nextSiblings要比childNodes效率高得多，在低版IE中。
children  childNodes
childElementCount  childNodes.length
firstElementChild firstChild
lastElementChild  lastChild
nextElementSibling nextSibling
previousElementSibling previousSibling
虽然查找只是比不区分节点的API快一点，但是一旦我们把他应用到这些遍历元素访问元素效率就会很明显


选择器API id结合class的方法效率低，且不方便，最好类似于css选择器的这种操作，方便快捷。
而且这种方法 返回值是nodeList，并不是HTML的集合，不会对应实时的文档结构。
document.querySelectorAll("#menu a，div.nodelist")
document.querySelector()返回第一个匹配的节点



浏览器的重绘重排
浏览器下载完所有的组件  -- html css  js image会生成DOM树和渲染树




