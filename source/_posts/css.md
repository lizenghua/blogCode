---
title: 常用css系列（持续更新···）
tags:
 - css
---

### 1. 单行超出文本出现省略号
```css
.text{
    white-space:nowrap; 
    text-overflow:ellipsis; 
    overflow:hidden; 
    word-break:break-all;
}
```

### 2. 多行超出文本出现省略号
```CSS
.text{
    display: -webkit-box;
    -webkit-box-orient:vertical; //检索伸缩盒对象的子元素的排列方式
    -webkit-line-clamp:3; //行数
    overflow:hidden;
    word-break:break-all; 
    white-space:normal;
}
```
> 注意：这种方法适合于WebKit浏览器及移动端，对于IE浏览器兼容不好

### 3. 背景透明，文字不透明
```CSS
.text{
    background:rgba(255, 255, 255, 0.75)!important;
    filter:Alpha(opacity=75); 
}
```
<!-- more -->
### 4. 处理pc图片比例自适应
```html
<!--HTML-->
<div class="image">
    <img src=""/>
</div>
```

```css
.img{
    .image{ width:130px; height:130px; position:relative;}
    .image img{ max-width:130px; max-height:130px; 
    position:absolute; left:0; right:0; top:0; bottom:0; width:auto; height:auto; margin:auto;}
}
```

### 5. css3实现逐帧动画
```html
<!--HTML-->
<div class="play-tip-btn"></div>
```
```css
.play-tip-btn{
    width: 136px;
    height: 88px;
    background: url(./images/play-btn.png) no-repeat; //改成你自己的图片地址
    -moz-animation: circle-anim 2s steps(30) infinite; // 这里的steps中的值为你动画图中有多少张画面
    -webkit-animation: circle-anim 2s steps(30) infinite;
    -o-animation: circle-anim 2s steps(30) infinite;
    -ms-animation: circle-anim 2s steps(30) infinite;
    animation: circle-anim 2s steps(30) infinite;
    cursor: pointer;
}
@-webkit-keyframes circle-anim {
    100% {
        background-position: -4080px; //width:136 * steps(30)
    }
}

@-moz-keyframes circle-anim {
    100% {
        background-position: -4080px;
    }
}

@-ms-keyframes circle-anim {
    100% {
        background-position: -4080px;
    }
}

@-o-keyframes circle-anim {
    100% {
        background-position: -4080px;
    }
}

@keyframes circle-anim {
    100% {
        background-position: -4080px;
    }
}
```

### 6.对齐两端文本 text-align-last: justify;
```html
<div class="bruce flex-ct-x">
    <ul class="justify-text">
        <li>账号</li>
        <li>密码</li>
        <li>电子邮件</li>
        <li>通讯地址</li>
    </ul>
</div>
```

```scss
$red: red;
.justify-text {
    li {
        margin-top: 5px;
        padding: 0 20px;
        width: 100px;
        height: 40px;
        background-color: $red;
        line-height: 40px;
        text-align-last: justify;
        color: #fff;
        &:first-child {
            margin-top: 0;
        }
    }
}
```

