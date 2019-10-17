## Week 1 - Day 1
### HTML
Programming language for document layout  
*Hyper Text Markup Language*  
Very simple language   
Web browsers are sympathetic compilers - tries to make it work even with errors  
One thing broken doesn’t mean it all is  
C++ is for programmers, HTML is for general public  

All about formatting. Doesn’t have loops, can’t calculate averages, etc.  
Text files with .html extension.  

HTML has one syntax element: **The Tag**  

```<tagName attr1=“value” attr2=“value">Other stuff</tagName>```

There are self closing tags: ```<tag />```  
First line of HTML is usually: ```<!DOCTYPE html>```

Common tags:

* ```<html>``` wraps all other tags
* ```<head>``` metadata for the document. Title, character encoding, 
* ```<body>``` all page content
* ```<!-- Comments like this -->```
* ```<h1> <h2> … <h6>``` headers. Big/Bold font types
* ```<p>``` paragraph
* ```<img />``` images, self closing, important to include attributes to allow image to be found (src=“pic.jpg”) alt=“pic of...” - when the image doesn’t load or cannot be viewed
* ```<a>``` anchor tag - this is a hyperlink tag. Not always text, sometimes images has to include href=“www.linkaddress.com” 
* ```<ul>``` is unordered list bullet points>
* ```<ol>``` numbered in order list
* ```<li>``` is the individual items in the lists
* ```<div>``` groups tags together

### CSS
*Cascading Style Sheets*  
Written in a .css file  

```<link rel=“stylesheet” type=“text/css” href=“myStyles.css” />``` goes in head of html file

```
Selector{ //what tag
    Attribute : value;
}

P{
This is applied to every p tag in the html file
}
```

To assign to similar grouped items, set a class in the tag on the html file

```
<p class=“myClass”>
.myClass{
Applies to all myClass html items
}
```

To assign a specific selector with css to an html element, set an id in the html
Ids should be unique, assigned to only ONE html element

```
<img id=“importantpicture” />
#importantpicture{
Applies to only the important picture item
}
```
