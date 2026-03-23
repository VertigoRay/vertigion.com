---
layout: post
title: 'Firefox Bookmarks Toolbar: Icons Only'
author: VertigoRay
img: //i.imgur.com/EGaOxxn.jpg
comments: true
tags:
- Firefox
- Bookmarks
---
Ever want to get rid of the text next to the icons on your bookmarks toolbar in Firefox?

I was looking over a co-workers shoulder the other day and noticed that his bookmarks toolbar had no text next to the icons!!
I immediately got as giddy as school girl and demanded enlightenment.
He promptly responded me that I merely needed to needed to save the following code in a `userChrome.css` file:

```js
toolbar\[mode="icons"\] .toolbarbutton-text {
   display:none !important;
}
```

The file needs to be saved in: `*%AppData%\Mozilla\Firefox\Profile\s########.defaultchrome`

- `########` = 8 random numbers and letters. ie: `h98nvfn5`

Not sure where your `%AppData%` folder is?
Try running the following from the Command Prompt: `echo %AppData%`

That should do it!

For more information, check out the [Customizing Mozilla](http://www.mozilla.org/unix/customizing.html) page!