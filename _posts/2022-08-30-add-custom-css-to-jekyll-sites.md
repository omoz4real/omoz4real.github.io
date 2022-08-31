---
layout: post
title: "Add custom css to jekyll site "
date: 2022-08-30 13:04:00 
---
To add custom css to a jekyll site, I created an assets folder in the root directory of the site and created a main.scss file in the assets folder
to add the custom css. Edit the content of the main.scss file and add the following front matter code and import statement to import the site theme's css file.

```ruby
---
---

@import " {{ site.theme }} "; 
 
```

Add all custom css code below the @import statement.

The version of Jekyll is 4.3.3 and Ruby 3.1.2.