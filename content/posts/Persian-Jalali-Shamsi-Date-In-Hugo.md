---
title: "تاریخ شمسی یا جلالی (ایرانی) در هوگو"
date: 2021-09-19T20:30:18+04:30
draft: false
tags: [تمپلیت, هوگو, اکسل, تاریخ-شمسی, تاریخ-جلالی, Hugo, Template-language]
tweet: https://twitter.com/mer30hamid/status/1439633119196303363
---

وقتی داشتم [وبلاگ شخصی خودم](https://www.hamid-kord.ir/) رو با هوگو درست می کردم، گفتم بیام بدون استفاده از جاوا اسکریپت و فقط با استفاده از زبان تمپلیت خود هوگو (که البته همون [زبان تملیت گو](https://gohugo.io/templates/introduction/) هست) برای هر پست، تاریخ رو بصورت شمسی یا همون جلالی نشون بدم، خب ایده این بود که با استفاده از یه فرمولی که قبلا توی اکسل پیدا کرده بودم، همون ایده استفاده از [زمان Unix](https://fa.wikipedia.org/wiki/%D8%B3%D8%A7%D8%B9%D8%AA_%DB%8C%D9%88%D9%86%DB%8C%DA%A9%D8%B3) رو توی هوگو هم استفاده کنم، برای اینکه ببینید تبدیل تاریخ در فرمول اکسل چطوری کار می کنه، این کد رو توی یه سلول اکسل قرار بدین (فرفی نمی کنه که اکسل مایکروسافت باشه یا اکسل گوگل داکس، فقط این فرمول رو توی یه سلول بگذارید)



```javascript
=CONCATENATE(INT((TODAY()-7385)/365.25)+1299,"/",MOD(IF(INT(MOD((TODAY()-7385)*100,36525)/100)<186,INT(INT(MOD((TODAY()-7385)*100,36525)/100)/31),IF(MOD(INT((TODAY()-7385)/365.25),4)=0,INT((INT(MOD((TODAY()-7385)*100,36525)/100)-186)/30)+6,IF(INT(MOD((TODAY()-7385)*100,36525)/100)<336,INT((INT(MOD((TODAY()-7385)*100,36525)/100)-186)/30)+6,INT((INT(MOD((TODAY()-7385)*100,36525)/100)-336)/29)+11))),12)+1,"/",IF(INT(MOD((TODAY()-7385)*100,36525)/100)<186,MOD(INT(MOD((TODAY()-7385)*100,36525)/100),31)+1,IF(MOD(INT((TODAY()-7385)/365.25),4)=0,MOD(INT(MOD((TODAY()-7385)*100,36525)/100)-186,30)+1,IF(INT(MOD((TODAY()-7385)*100,36525)/100)<336,MOD(INT(MOD((TODAY()-7385)*100,36525)/100)-186,30)+1,MOD(INT(MOD((TODAY()-7385)*100,36525)/100)-336,29)+1))))
```

خروجی با فرمت YYYY/MM/DD نمایش داده میشه یعنی از سمت چپ سال، ماه و روز. حالا کد تمپلیت هوگو از فرمول بالا این میشه:

```twig
{{ $shamsi := newScratch }}
{{ $shamsi.Set "diff" (sub (add ( div .PublishDate.Unix 86400) 25569) (add ( div (time "1920-03-20").Unix 86400) 25569)) }}
{{ $shamsi.Set "gn" (div (mod (mul ($shamsi.Get "diff") 100) 36525) 100) }}
{{ $shamsi.Set "year" (math.Floor (add (div ($shamsi.Get "diff") 365.25) 1299)) }}

{{ if lt (int ($shamsi.Get "gn")) 186 }}
  {{$shamsi.Set "month" (add (mod (int (div ($shamsi.Get "gn") 31)) 12) 1)}}
{{else if eq (mod (int (div ($shamsi.Get "diff") 365.25)) 4) 0}}
  {{ $shamsi.Set "month" ( add (mod (add (int (div (sub (int ($shamsi.Get "gn")) 186) 30)) 6) 12) 1) }}
{{else if lt (int ($shamsi.Get "gn")) 336 }}
  {{ $shamsi.Set "month" ( add (mod (add (int (div (sub (int ($shamsi.Get "gn")) 186) 30)) 6) 12) 1) }}
{{else}}
  {{ $shamsi.Set "month" ( add (mod (add (int (div (sub (int ($shamsi.Get "gn")) 336) 29)) 11) 12) 1) }}
{{ end }}

{{ if lt (int ($shamsi.Get "gn")) 186 }}
  {{ $shamsi.Set "day" (add (mod (int ($shamsi.Get "gn")) 31) 1) }}
{{else if eq (mod (int (div ($shamsi.Get "diff") 365.25)) 4) 0}}
  {{ $shamsi.Set "day" (add (mod (sub (int ($shamsi.Get "gn")) 186) 30) 1) }}
{{else if lt (int ($shamsi.Get "gn")) 336 }}
  {{ $shamsi.Set "day" (add (mod (sub (int ($shamsi.Get "gn")) 186) 30) 1) }}
{{else}}
  {{ $shamsi.Set "day" (add (mod (sub (int ($shamsi.Get "gn")) 336) 29) 1) }}
{{ end }}
{{ $shamsi.Get "year" }}/{{$shamsi.Get "month"}}/{{$shamsi.Get "day"}}
```

راستش نمی دونم تا چه حد دقیق تونستم تبدیلش کنم، حالا [این گیست رو هم گذاشتم](https://gist.github.com/mer30hamid/e178b58f3a0959e3a4b40d12c74d7cf4)، اگه بهبودی میشه داد، تغییر رو بهم بگید.





