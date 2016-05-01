#gulp-bracks

[![npm](https://img.shields.io/npm/v/gulp-bracks.svg?maxAge=2592000)]() 
[![npm](https://img.shields.io/npm/dt/gulp-bracks.svg?maxAge=2592000)]() 

bracks plugin for [gulp](https://github.com/gulpjs/gulp). If you don't know what `bracks` style document is, please read [bracks-parser](https://github.com/mawni/nodejs-bracks-parser).
#####Install
`npm install gulp-bracks --save-dev`
#####How to use
If you want to write your `html` or `ejs` files `bracks` style, just create a directory under your project root directory and name it `bracks`. Then, keep all the `html` or `ejs` files that you want to write in a `bracks` syntax in this direcory. Files can be located in sub-direcories in this `bracks` directory, just notice the pattern for the gulp src path in the following `gulpfile.js` example. Also, notice the file extension can be `.html` or `.ejs`. `bracks-parser` understands both of them. If it is needed, the src path can be set like `gulp.src('./bracks/**/*.+(html|ejs)')` in order to get both `.html` and `.ejs` file extensions. Under the hood, the parser parses all files under `bracks` directory, and returns the transformed file for being used by the next function down in the stream. So, if gulp watches the `bracks` directory (similar to the following example), as you change your `bracks` files, their corresponding html||ejs formatted documents are dynamically being overwritten and updated.

Something like the following will do the job for you.

*gulpfile.js*
```javascript
var gulp = require('gulp');
var parse_bracks = require('gulp-bracks');

gulp.task('bracks', function() {
  return gulp.src('./bracks/**/*.html')
    .pipe(parse_bracks())
    .pipe(gulp.dest('./'));
});

gulp.task('watch', function() {
  gulp.watch('./bracks/**/*.html', ['bracks']);
});

gulp.task('default', ['bracks', 'watch']);
```
#####Example of a `bracks` style html document
*index.html*:
```
<!DOCTYPE html>
html[
  c/[your comment]/c
  head[title[your page title]title]head
  body[
    h1[explore your mind]h1
    b[your bold text]b
    div(id="yourdiv" class="yourdivclass")[
      a(href="https://www.google.com")[link to google]a
      ul(style="list-style-type:disc")[
        li[item1]li
        li[item2]li
        li[a(href="https://www.google.com")[link to google]a]li
      ]ul
    ]div
    div[
      p[it is a samp s script footer paragraph tester]p
    ]div
  ]body
]html
```
#####Example of a `bracks` style ejs document
*index.ejs*:
```
<!DOCTYPE html>
html[
  head[
    title[your page title]title
    link(rel="stylesheet" href="/stylesheets/style.css")/]
    meta(charset="utf-8")/]
  ]head
  body(class="%= page %]")[
    [% include partials/template/header.ejs %]
      section(class="layout")[
        div(class="primary")[
          %- include partials/content/home-page.ejs -%
        ]div
        p[explore your mind]p
        aside(class="secondary")[
          %- include partials/content/proj-page.ejs %]
        ]aside
      ]section
    [% include partials/template/footer.ejs %]
  ]body
]html
```
