---
layout: post
title: "Add custom css to jekyll site "
date: 2022-08-30 
---
To add custom css to a jekyll site, Create a new folder assets in the project root directory of the site. Then create a main.scss file in the assets folder
to add the custom css. Edit the content of the main.scss file and add the following code to import the current default site theme's css file.

```ruby
---
---

@import " {% raw %}{{ site.theme }} {% endraw %}"; 
 
```

Add all custom css code below the @import statement.

The version of Jekyll is 4.3.3 and Ruby 3.1.2.

[EappsJelastic Platform as a Service](https://portal.eapps.com/aff.php?aff=2289 "Eapps Jelastic PaaS"){:target="_blank"}