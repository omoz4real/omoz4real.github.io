---
layout: post
title: "Add Padding to ion-content in ionic5"
date: 2021-04-29
---
To add padding around ion-content component in ionic 5, Add the following CSS custom properties in the SCSS file associated with the page. For example to add 
padding around the component faq.page.html, simply update the faq.page.scss with the following custom css

```css
 ion-content {
  --padding-bottom: 10px;
  --padding-end: 10px;
  --padding-start: 15px;
  --padding-top: 15px;
}
```

The css code above adds padding to your content area in ionic 5.