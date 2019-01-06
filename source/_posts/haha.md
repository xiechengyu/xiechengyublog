---
title: haha
date: 2019-01-06 20:43:46
tags:
---

# 前言

因为公司使用的css编译工具是sass，所以我就自学了sass，在阅读了很多文章之后，发现有一篇文章对于sass语法的总结比较全面好用，我就把它搬运过来了,让自己在查找资料时变的不那么费劲了。

文章相关内容转载自：[sass语法总结](https://www.jianshu.com/p/e139d449f5bb)

作者：xoyoz

來源：简书

<!--more-->
## scss 和 sass语法的相互转换

```css
# Convert Sass to SCSS
$ sass-convert style.sass style.scss

# Convert SCSS to Sass
$ sass-convert style.scss style.sass
```

# 编译

## 命令行编译

```css
#单文件
sass --watch input.scss:output.css
#多文件
sass --watch app/sass:public/stylesheets

# --wacth 自动编译
```

## GUI 界面工具编译

[Koala](https://github.com/xiechengyu)

[Compass.app](https://link.jianshu.com/?t=http://compass.kkbox.com/)

[Scout](https://link.jianshu.com/?t=http://mhs.github.io/scout-app/)

[CodeKit](https://link.jianshu.com/?t=https://incident57.com/codekit/index.html)

[Prepros](https://link.jianshu.com/?t=https://prepros.io/)

## 自动化编译

Grunt 配置 Sass 编译的示例代码

```js
module.exports = function(grunt) {
    grunt.initConfig({
        pkg: grunt.file.readJSON('package.json'),
        sass: {
            dist: {
                files: {
                    'style/style.css' : 'sass/style.scss'
                }
            }
        },
        watch: {
            css: {
                files: '**/*.scss',
                tasks: ['sass']
            }
        }
    });
    grunt.loadNpmTasks('grunt-contrib-sass');
    grunt.loadNpmTasks('grunt-contrib-watch');
    grunt.registerTask('default',['watch']);
}
```

Gulp 配置 Sass 编译的示例代码

```js
var gulp = require('gulp');
var sass = require('gulp-sass');

gulp.task('sass', function () {
    gulp.src('./scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./css'));
});

gulp.task('watch', function() {
    gulp.watch('scss/*.scss', ['sass']);
});

gulp.task('default', ['sass','watch']);
```

## 编译错误

1.要用UTF-8
 2.文件路径及文件名不要用中文

## 输出方式

1.嵌套输出方式 nested

```css
$ sass --watch test.scss:test.css --style nested
```

编译出来的CSS风格

```css
 nav ul {
  margin: 0;
  padding: 0;
  list-style: none; }
nav li {
  display: inline-block; }
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none; }
```

2.展开输出方式 expanded（默认样式）

```css
$ sass --watch test.scss:test.css --style expanded
```

编译出来的CSS风格

```css
nav ul {
  margin: 0;
  padding: 0;
  list-style: none;
}
nav li {
  display: inline-block;
}
nav a {
  display: block;
  padding: 6px 12px;
  text-decoration: none;
}
```

3.紧凑输出方式 compact

```css
sass --watch test.scss:test.css --style compact
```

编译出来的CSS风格

```css
nav ul { margin: 0; padding: 0; list-style: none; }
nav li { display: inline-block; }
nav a { display: block; padding: 6px 12px; text-decoration: none; }
```

4.压缩输出方式 compressed

```css
sass --watch test.scss:test.css --style compressed
```

编译出来的CSS风格

```css
nav ul{margin:0;padding:0;list-style:none}nav li{display:inline-block}nav a{display:block;padding:6px 12px;text-decoration:none}
```

# 基础

## 变量

### 声明变量

sass离不开$

```css
$width : 300px;
```

### 普通变量 & 默认变量

```css
$fontSize: 12px;
body{
    font-size:$fontSize;
}
$baseLineHeight:1.5 !default;
body{
    line-height: $baseLineHeight; 
}
```

### 局部变量 & 全局变量（3.4+）

```css
//SCSS
$color: orange !default;//定义全局变量
.block {
  color: $color;//调用全局变量
}
em {
  $color: red;//定义局部变量（全局变量 $color 的影子）
  a {
    color: $color;//调用局部变量
  }
}
```

## 嵌套

### 选择器嵌套

```css
nav {
  a {
    color: red;

    header & {
      color:green;
    }
  }  
}
nav a {
  color:red;
}

header nav a {
  color:green;
}
```

其中`&`代表其所在位置的所有长辈元素

### 属性嵌套

主要用于padding ，margin，font等属性

```css
.box {
  border: {
   top: 1px solid red;
   bottom: 1px solid green;
  }
}
.box {
    border-top: 1px solid red;
    border-bottom: 1px solid green;
}
```

### 伪类嵌套

```css
.clearfix{
&:before,
&:after {
    content:"";
    display: table;
  }
}
clearfix:before, .clearfix:after {
  content: "";
  display: table;
}
```

## 混合宏

### 声明混合宏@mixin

1.不带参数的混合宏

```css
@mixin border-radius{
    -webkit-border-radius: 5px;
    border-radius: 5px;
}
```

2.带参数的混合宏

```css
//一个参数时，可以指定默认值
@mixin border-radius($radius:5px){//此时的5px为默认参数，传入其他值时覆盖该值
    -webkit-border-radius: $radius;
    border-radius: $radius;
}
//传2个参数
@mixin center($width, $height){
...
}
//参数过多 ，一个特别的参数 （...）
@mixin box-shadow($shadows...){
  @if length($shadows) >= 1 {
    -webkit-box-shadow: $shadows;
    box-shadow: $shadows;
  } @else {
    $shadows: 0 0 2px rgba(#000,.25);
    -webkit-box-shadow: $shadow;
    box-shadow: $shadow;
  }
}
```

3.复杂的混合宏

```css
@maxin box-shadow($shadow..){
    @if length($shadow) >= 1{
        @include prefixer(box-shadow, $shadow);
    } @else {
        $shadow:0 0 4px rgba(0,0,0,.3);
        @include prefixer(box-shadow, $shadow);
