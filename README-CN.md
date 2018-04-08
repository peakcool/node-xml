node-xml
===================

(C) Rob Righter (@robrighter) 2009 - 2010, Licensed under the MIT-LICENSE
Contributions from David Joham

 node-xml is an xml parser for node.js written in javascript.

<!-- TOC -->

- [安装](#安装)
- [SAX Parser](#sax-parser)
    - [new xml.SaxParser()](#new-xmlsaxparser)
    - [new xml.SaxParser(callback)](#new-xmlsaxparsercallback)
- [Parse](#parse)
    - [parser.parseString(string)](#parserparsestringstring)
    - [parser.parseFile(filename)](#parserparsefilefilename)
    - [parser.pause()](#parserpause)
    - [parser.resume()](#parserresume)
- [Callbacks](#callbacks)
    - [parser.onStartDocument(function() {})](#parseronstartdocumentfunction-)
    - [parse.onEndDocument(function() {})](#parseonenddocumentfunction-)
    - [parser.onStartElementNS(function(elem, attrs, prefix, uri, namespaces) {})](#parseronstartelementnsfunctionelem-attrs-prefix-uri-namespaces-)
    - [parser.onEndElementNS(function(elem, prefix, uri) {})](#parseronendelementnsfunctionelem-prefix-uri-)
    - [parser.onCharacters(function(chars) {})](#parseroncharactersfunctionchars-)
    - [parser.onCdata(function(cdata) {})](#parseroncdatafunctioncdata-)
    - [parser.onComment(function(msg) {})](#parseroncommentfunctionmsg-)
    - [parser.onWarning(function(msg) {})](#parseronwarningfunctionmsg-)
    - [parser.onError(function(msg) {})](#parseronerrorfunctionmsg-)

<!-- /TOC -->

# 安装

```
npm install node-xml
```

API
---
 

SaxParser
---------

Node-xml 提供`SAX2`可以接受一个字符串文件的解析器接口。解析器可以从文档中以字块形式获取字符。将文档发送给解析器可以使用`parsestring(xml)`。

# SAX Parser

## new xml.SaxParser()
	* 实例化 SaxParser
	* 返回: SaxParser对象

## new xml.SaxParser(callback)
	* 实例化 SaxParser
	* 返回: SaxParser对象
	* 参数
		* callback - 回调方法
	
# Parse

## parser.parseString(string)

解析字符串
* 返回： true/false
* 参数：string - 需要解析的字符串

## parser.parseFile(filename)

解析文件
* 返回： true/false
* 参数：string - 需要解析的文件字符串
	
## parser.pause()

暂停解析

## parser.resume()

恢复解析

# Callbacks

## parser.onStartDocument(function() {})

开始解析时调用

## parse.onEndDocument(function() {})

介绍解析时调用

## parser.onStartElementNS(function(elem, attrs, prefix, uri, namespaces) {})

标签开始时
* 参数
    * `elem` - 表示元素名称的字符串
    * `attrs` - 二维数组类型: `[[key, value], [key, value]]`
    * `prefix` - 表示元素名称空间前缀的字符串
    * `uri` - 元素命名空间
    * `namespaces` - 二维数组 `[[prefix, uri], [prefix, uri]]`

## parser.onEndElementNS(function(elem, prefix, uri) {})

标签结束时
* 参数
    * `elem` - 表示元素名称的字符串
    * `prefix` - 表示元素名称空间前缀的字符串
    * `uri` - 元素命名空间


## parser.onCharacters(function(chars) {})

当解析内容为一组内容字符时调用
* 参数
    * chars - 字符串

## parser.onCdata(function(cdata) {})

当解析内容为CDATA时调用
* 参数
    * cdata - 表示CDATA的字符串

## parser.onComment(function(msg) {})

当解析内容为注释时调用
* 参数
    * msg - 表示注释的字符串

## parser.onWarning(function(msg) {})

当解析警告时调用
* 参数
    * msg - 表示警告消息的字符串

## parser.onError(function(msg) {})

当解析错误时调用
* 参数
    * msg - 表示错误消息的字符串
	
    

示例
-------------

```js
var util = require('util');
var xml = require("./lib/node-xml");

var parser = new xml.SaxParser(function(cb) {
	cb.onStartDocument(function() {
	
	});
	cb.onEndDocument(function() {
	
	});
	cb.onStartElementNS(function(elem, attrs, prefix, uri, namespaces) {
		util.log("=> Started: " + elem + " uri="+uri +" (Attributes: " + JSON.stringify(attrs) + " )");
	});
	cb.onEndElementNS(function(elem, prefix, uri) {
		util.log("<= End: " + elem + " uri="+uri + "\n");
			parser.pause();// pause the parser
			setTimeout(function (){parser.resume();}, 200); //resume the parser
	});
	cb.onCharacters(function(chars) {
		//util.log('<CHARS>'+chars+"</CHARS>");
	});
	cb.onCdata(function(cdata) {
		util.log('<CDATA>'+cdata+"</CDATA>");
	});
	cb.onComment(function(msg) {
		util.log('<COMMENT>'+msg+"</COMMENT>");
	});
	cb.onWarning(function(msg) {
		util.log('<WARNING>'+msg+"</WARNING>");
	});
	cb.onError(function(msg) {
		util.log('<ERROR>'+JSON.stringify(msg)+"</ERROR>");
	});
});


//example read from chunks
parser.parseString("<html><body>");
parser.parseString("<!-- This is the start");
parser.parseString(" and the end of a comment -->");
parser.parseString("and lots");
parser.parseString("and lots of text&am");
parser.parseString("p;some more.");
parser.parseString("<![CD");
parser.parseString("ATA[ this is");
parser.parseString(" cdata ]]>");
parser.parseString("</body");
parser.parseString("></html>");

//example read from file
parser.parseFile("sample.xml");
```
