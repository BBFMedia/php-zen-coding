#summary Basic Usage

##Introduction

A simple php parser of zen coding statement. [http://code.google.com/p/zen-coding/]

Many features of zen coding is not supported. Templates such as input:image  etc. will be supported shortly.

##Examples

```php
    require_once "zencoding.php";
    echo zenCode::render('div>ul>li>p["text"]+p["text two"]');
```


A basic usage

```php
	$html = zenCode::render('div>ul>li>p["text"]+p["text two"]');
```
	
```xml
	<div><ul><li><p>test</p><p>text two</p></li></ul></div>
```

You can pass an array of data and refence with `{}`. The example also show the enumerator with *3

```php
    $var = array('hello'=>'ni hao', 'test' => array('hello', 'world'));
    $html = zenCode::render('div>ul>li[{hello}]+li["{test[$]}"]*2', $var);
```

```xml
    <div><ul><li>ni hao</li><li>hello</li><li>word</li></ul></div>
```

```php
    $html = zenCode::render('a#test.thisclass.otherclass[ref="{test[0]}"]["content text "]>ul>li#$[href="{test[$]}link"]*2', $var);
```


With user functions referenced in an array.
```php
    function myfunc1($str)
    {
        return ucfirst($str);
    }
    function myfunc2($index)
    {
        return 'This '.$index;
    }
    $var = array('a_function1'=>'myfunc1','a_function2'=>'myfunc2');
    $html = zenCode::render('div["{a_function1("test")}+div["{a_function2($)}"]*3',$var);
```


Inline Code. Inline code is wrapped with  `{= }`

```php
    $var = array('test' => array('hello', 'world'));

    $html = zenCode::render('div["{=  return $test[0] . " - ". $test[1]; }"]',$var);
```

```php
    $html = zenCode::render('div["{=  $x = $test[0]; $y = $test[1]; return $x . " - " . $y; }"]',$var);
```

###Supported Zen HTML Selectors

This list is directly from the zencoding [http://code.google.com/p/zen-coding/wiki/ZenHTMLSelectorsEn].
##E#name
###div#name


	<div id="name"></div>
 
`E.name`
 div.name`

    <div class="name"></div>
 
`div.one.two`

    <div class="one two"></div>
 
`div#name.one.two`

    <div id="name" class="one two"></div>
 
`E>E`

`ul#name>li.item`


    <ul id="name">
        <li class="item"></li>
    </ul>

`E+E`
`p+p``


	
<p></p>
<p></p>

`div#name>p.one+p.two`

    <div id="name">
        <p class="one"></p>
        <p class="two"></p>
    </div>

`E[attr]`


`p[title]`

    <p title=""></p>

`td[colspan=2]`

    <td colspan="2"></td>

`span[title="Hello" rel]`

    span title="Hello" rel=""></span>


`E|filter`

Not supported

`E*N`
p*3

    <p></p>
    <p></p>
    <p></p>

`ul#name>li.item*3`

    <ul id="name">
        <li class="item"></li>
        <li class="item"></li>
        <li class="item"></li>
    </ul>

`E*N$`
`p.name-$*3`

    <p class="name-1"></p>
    <p class="name-2"></p>
    <p class="name-3"></p>

`select>option#item-$*3`

    <select>
        <option id="item-1"></option>
        <option id="item-2"></option>
        <option id="item-3"></option>
    </select>
 

`E+
ul+
table+
dl+`

Not supported