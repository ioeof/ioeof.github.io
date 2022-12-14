# Ioeof is a simple HTML + Javascript Timeline.

It's easy to make a **Timeline**, it can also be used as a **TabPage** component, whatever you want.

## CSS

```CSS
.Ioeof{display:flex;flex-wrap:nowrap;white-space:nowrap;position:relative;overflow:hidden;overflow-x:auto;user-select:none;padding-bottom:10px;}
.Ioeof::-webkit-scrollbar{width:6px;height:6px;}
.Ioeof::-webkit-scrollbar-thumb{background:#ccc;border-radius:3px;}
.Ioeof::-webkit-scrollbar-track{background:transparent;}
.Ioeof>.IoeofEdge>div{width:0;height:1px;}
.Ioeof>.IoeofTab{position:relative;}
.Ioeof>.IoeofTab>.IoeofTabText{font-size:20px;line-height:36px;padding:0 16px;cursor:pointer;background:#ccc;border-radius:18px;}
.Ioeof>.IoeofTab:focus>.IoeofTabText{background:#eee;}
.Ioeof>.IoeofTab:hover>.IoeofTabText{background:#eee;}
.Ioeof>.IoeofTab.active>.IoeofTabText{background:#fff;}
```

## HTML

```html
<div id="IoeofTest" class="Ioeof">
    <div class="IoeofEdge"><div></div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2002</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2003</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2004</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2005</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2006</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2007</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2008</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2009</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2010</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2011</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2012</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2013</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2014</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2015</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2016</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2017</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2018</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2019</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2020</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2021</div></div>
    <div class="IoeofTab"><div class="IoeofTabText">2022</div></div>
    <div class="IoeofEdge"><div></div></div>
</div>
<div id="ClickTest" style="text-align:center;font:26px/36px tahoma;padding:20px;"></div>
```

## JS

```javascript
var Ioeof=(function(){var that=this;this.margin=0;this.selector='.Ioeof';this.callback=null;
    this.config=function(selector,margin){if(selector)this.selector=selector;if(margin){this.margin=margin;var l=document.querySelectorAll(String(this.selector)+'>.IoeofTab');for(var i=l.length-1;i>=0;i--)l[i].style.margin='0 '+String(this.margin)+'px';}return this;};
    this.center=function(width){var e=(width)?Math.round((document.querySelector(this.selector).getBoundingClientRect().width-width-this.margin*2)/2):0;var l=document.querySelectorAll(String(this.selector)+'>.IoeofEdge>div');for(var i=l.length-1;i>=0;i--)l[i].style.width=String(e)+'px';return this;};
    this.handler=function(){var l=document.querySelectorAll(String(that.selector)+'>.IoeofTab.active');for(var i=l.length-1;i>=0;i--)l[i].classList.remove('active');var dom=this;dom.classList.add('active');if(that.callback)try{that.callback(dom,dom.dataset['index'],that);}catch(e){}};
    this.onclick=function(callback){this.callback=callback;var l=document.querySelectorAll(String(this.selector)+'>.IoeofTab');for(var i=l.length-1;i>=0;i--){l[i].dataset['index']=i;l[i].onclick=this.handler;}return this;};
    this.mobile=function(){return navigator.userAgent.toLowerCase().indexOf('mobile')>0;};
    this.focus=function(index){index=parseInt(index);if(isNaN(index))return null;if(index<0)return null;var l=document.querySelectorAll(String(this.selector)+'>.IoeofTab');if(index>=l.length)return null;var ioeof=document.querySelector(this.selector);var dom=l[index];var w=ioeof.getBoundingClientRect().width;var v=dom.getBoundingClientRect().width;var l=ioeof.scrollLeft;var e=dom.offsetLeft;if(!((e>=l)&&(e+v<=l+w)))ioeof.scrollLeft=Math.round(e+v/2-w/2);return dom;};
    this.click=function(index){var dom=this.focus(index);if(!dom)return null;this.handler.apply(dom);return dom;};
    this.count=function(){return document.querySelectorAll(String(this.selector)+'>.IoeofTab').length;};
    this.draggable=function(){if(this.mobile())return this;var ioeof=document.querySelector(this.selector);ioeof.onmousedown=function(e){ioeof.classList.add('dragging');ioeof.dataset['dragstart']=e.pageX-ioeof.offsetLeft;ioeof.dataset['dragscroll']=ioeof.scrollLeft;};ioeof.onmousemove=function(e){if(!ioeof.classList.contains('dragging'))return;e.preventDefault();ioeof.scrollLeft=ioeof.dataset['dragscroll']-(e.pageX-ioeof.offsetLeft-ioeof.dataset['dragstart'])*1;};ioeof.onmouseup=function(e){ioeof.classList.remove('dragging');};ioeof.onmouseleave=function(e){ioeof.classList.remove('dragging');};return this;};
});
```

```javascript
var ioeof=new Ioeof().config('#IoeofTest'/*Selector*/,6/*Tab Margin Width*/)
    .draggable()/*if you need Horizontal Drag Scrolling*/
    .onclick(function(tab,index,self){
        /*tab click event*/
        document.querySelector('#ClickTest').innerHTML='#'+String(index)+' tab is activated : <b>'+tab.textContent+'</b>';
    });
/*ioeof.center(960);/*if you need to set a center width*/
if(ioeof.count())ioeof.click(0);/*manual click tab*/
```
