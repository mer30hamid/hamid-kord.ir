
---
published: true
title: راه اندازی سرور DoH اختصاصی فقط با PHP
layout: post
tags: [برنامه_نویسی, شبکه, DNS, DoH]
date: 2022-08-04T17:15:06+04:30
tweet: https://twitter.com/mer30hamid/status/1555177749937065988
---

این روزها خیلی حرف سر اینه که DNS یه پروتکل قدیمی و نا امن شده و توی نسخه های جدید سیستم عامل ها و مرورگرها به این فکر افتادن که بیان فکری به حالش بکنن.
توی اینترنت میگشتم ببینم روشی هست که بشه یه سرور DoH راه انداخت که پیچیدگی نداشته باشه و بتونیم با اسکریپت php اون رو روی هر هاستی بیاریم بالا تا اینکه رسیدم به یک اسکریپت جالب که  یه نفر اومده بود RFC ـه خود DoH رو خونده بود و خودش اونو به زبان پی اچ پی نوشته بود. من نگاش کردم  و یه تغییر کوچکی دادم و ازش جواب گرفتم، حالا تغییر کوچیک من چی بود؟ هیچی! فقط اومدم سروری که قراره بهش وصل بشه رو عوض کردم. خودش پیشنهاد داده بود که به سرور کلادفلر وصل بشه من جاش گوگل رو نوشتم:

```php
<?php
if (isset($_SERVER['CONTENT_TYPE']) && $_SERVER['CONTENT_TYPE'] == 'application/dns-message')
{
        $request = file_get_contents("php://input");
        header("Content-Type: application/dns-message");
        $s = fsockopen("udp://8.8.8.8", 53, $errno, $errstr);
        if ($s)
        {
                fwrite($s, $request);
                echo fread($s, 4096);
                fclose($s);
        }
}
else if (isset($_GET['dns']))
{
        $request = base64_decode(str_replace(array('-', '_'), array('+', '/'), $_GET['dns']));
        header("Content-Type: application/dns-message");
        $s = fsockopen("udp://8.8.8.8", 53, $errno, $errstr);
        if ($s)
        {
                fwrite($s, $request);
                echo fread($s, 4096);
                fclose($s);
        }
}
```

اسکریپت اصلی رو اینجا ببینید:
https://github.com/NotMikeDEV/DoH/blob/master/dns.php

یه توضیح مختصر بدم که این اسکریپت کارش چیه؟ در واقع این اسکریپت کار یک پراکسی رو انجام میده، یعنی درخواست DoH از کلاینت شما که بهش برسه، اون رو بصورت درخواست DNS معمولی میفرسته سمت سرور دی ان اسی که شما بهش گفتین، مثلا من گفتم گوگل 8.8.8.8 باشه و بعد جواب درست رو بر می گردونه. یعنی یه چیزی شبیه به این:

```
+----------+       +----------+                                                      
|DoH Client|------>|PHP Script|                                                      
+----------+       +----------+                                                      
                         |                                                           
                         |                                                           
                         |                                                           
                         v                                                           
                  +------------+                                                     
                  | DNS Server |                                                     
                  +------------+
```

فقط حتما یه نکته رو دقت کنید که باید هاستی که اسکریپت PHP رو میزبانی می کنه دارای HTTPS باشه که الان راحت می شه تهیه کرد.

پس مراحل رو برای انجام کار بصورت مختصر می گم:

1. اول یه هاست مناسب تهیه کنید. فرقی نمی کنه اشتراکی، رایگان و ... باشه، من خودم گذاشتم روی یه هاست رایگان و جواب داد، فقط مهمه که HTTPS داشته باشه و مهمتر این که خودش توی یه شبکه امن باشه (یعنی داخل کشوری باشه که پروتکل DNS رو دستکاری نمی کنه)
2. فایل اسکریپت رو با نام dns-query.php داخل فولدر ریشه هاست بگذارید، طوری که اگر مثلا آدرس سایت شما www.yourdomain.com باشه، باید بصورت زیر از اینترنت بشه بهش دسترسی داشت:
`www.yourdomain.com/dns-query.php`
3. داخل فایل `.htaccess` که اون هم داخل فولدر ریشه سایت هست، این کد رو قرار بدین، این برای اینه که پسوند php از آخر آدرس شما حذف بشه، چرا؟ چون داخل اکثر کلاینت ها نباید پسوند انتهای آدرس باشه


```htaccess
# .htaccess 
RewriteCond %{REQUEST_FILENAME}.php -f
RewriteRule !.*\.php$ %{REQUEST_FILENAME}.php [QSA,L]
```

حالا سرور شما آماده شده، کافیه اون رو تست کنید. برای تست داخل گوشی اندروید از [Intra](https://getintra.org/) استفاده کنید، کافیه توی قسمت تنظیمات برید و :
```
Select Dns Over HTTPS Server
```
رو انتخاب کنید و بعد آدرس خودتون رو (مثلا که گفتم) بصورت زیر
```
https://www.yourdomain.com/dns-query
```
وارد کنید و توی صفحه اصلی برنامه بهش وصل بشید، از اونجایی که این مطلب رو در مورد نحوه وصل شدن به DoH ننوشتم، توی اینترنت در مورد روش های وصل شدنش جستجو کنید، توی سیستم عامل ها هم داره استاندارد میشه و نیازی به برنامه های واسط نیست، مثلا آخرین نسخه های ویندوز 10 و ویندوز 11 بصورت استاندارد تنظیم DoH یا حتی DoT رو هم درست در جایی که تنظیم DNS شبکه رو میشه انجام داد، قبول می کنن، اندروید هم از نسخه 10 به بعد داخل تنظیمات شبکه اش این رو آورده.

در آخر بگم حتما به [صفحه اسکریپت](https://github.com/NotMikeDEV/DoH) سر بزنید و نکاتی که  اونجا گفته رو هم حواستون باشه، این روش برای درخواست های زیاد جوابگو نیست. 