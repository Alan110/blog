---
title: gulp搭建构建系统
date: 2016-08-21 16:26:19
type: "categories"
categories: "构建工具"
tags:
---


前端的3大缺陷能力

* 资源定位

* 内容嵌入

* 声明依赖

整个构建过程是输入->输出，
要么就是拆分完整的输入到输出流程,互相重叠不大的task，要么是一次性完整输入，然后筛选做不同处理，然后一次性完整输出


## 单文件处理

## 打包

以html页面为单位，打包资源，并添加md5

```javascript

gulp.task('ref',function(){
    gulp.src('unittest/mocha/index.html')
        .pipe(useref())
        .pipe(gulpif('*.js', rev()))
        .pipe(revReplace())
        .pipe(gulp.dest(distPath))
        .pipe(rev.manifest())
        .pipe(gulp.dest(revPath));
});

```

将打包后的文件变成单文件，然后做单文件处理



被打包的文件应该自行做单文件处理，和资源定位

## 注意事项

1. gulp的任务是异步执行的不能保证顺序，使用gulp-sequence同步执行任务

*  dest不能重命名，使用gulp-rename解决

*  最好分成开发配置，上线配置2分，互不干扰

*  插件太多，每个都要require很麻烦，使用gulp-load-plugins解决，$.pluginName , 驼峰命名

2. API

gulp.src

gulp.dest
