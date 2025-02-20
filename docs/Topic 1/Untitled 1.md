---
marp: true
theme: rose-pine
paginate: true
size: 4:3
title: Marp custom themes
---
<!-- _class: title -->

# A very long title of my beamer-esque presentation
<br/>

Author's name
University of XYZ
2022-26-03
(Only normal text is centered now)

---

# A normal slide



---
# Title page ad hoc fix

- If the title of your presentation is too long and the border intersects with the text underneath, use the following

```html
# Title
<br/>
<!-- empty line here --->
Author's name
University of XYZ
...
```
- make sure to leave an empty line below the `<br/>` tag

---
<!-- _class: tinytext -->
# Tinytext class

- use `<!-- _class: tinytext -->` to make some text tiny
- might be useful for References