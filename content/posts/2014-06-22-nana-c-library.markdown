---
published: true
title: آشنایی اولیه با Nana C++ Library!
Description : "معرفی یک کتابخانه جالب برای ساخت رابط کاربری"
layout: post
tags: [برنامه_نویسی, C++, Nana]
date: 2014-06-22T22:09:36+04:30
---
با وجود قدرت و زیبایی دستور زبان، در عمل کسی حاضر نیست سمت طراحی رابط کاربری یا GUI در زبان <i dir="ltr" style="direction: ltr;"><code>C++</code></i> برود، چرا که ساختن GUI در این زبان بسیار مشکل است. برخی از چارچوب های GUI در زبان <i dir="ltr" style="direction: ltr;"><code>C++</code></i> قوانین خودشان را دارند که باعث می شه شما مجبور به نوشتن کدهای سنگین برای آن شوید، و این امر همیشه سبب بروز برخی مشکلات مثل باقی ماندن کد اصلی شما در عمق سلسله مراتب وابستگی ها و ایجاد سختی در نگهداری آن می شود. حالا یک جایگزین پیدا شده، Nana C++ Library یک کتابخانه ناب از <i dir="ltr" style="direction: ltr;"><code>C++</code></i> که راهنمای شما در ایجاد رابط کاربری هست و دانش، مهارت و سبک و سیاق شما در <i dir="ltr" style="direction: ltr;"><code>C++</code></i> را حفظ می کند و فرایندی عالی را در طراحی رابط کاربری در زبان <i dir="ltr" style="direction: ltr;"><code>C++</code></i> فراهم می آورد.  <!--more-->

**آسان یاد بگیر، آسان استفاده کن**   
ایجاد یک برنامه Hello World با Nana چقدر آسونه؟
```c++
#include <nana/gui/wvl.hpp>
#include <nana/gui/widgets/label.hpp>

int main()
{
    using namespace nana::gui;
    form fm;
    label lb(fm, fm.size());
    lb.caption(L"Hello, World");
    fm.show();
    exec();
}
```

خیلی آسان، با کدهای قابل فهم. Nana از مفاهیم قابل درک و ساده برای آسان کردن این کار استفاده می کنه. در ثانی ، برخلاف چارچوب های دیگر، که باعث می شه شما با دشواری کد بنویسید که علتش قواعد و دستورالعمل های سخت در نامگذاری و مقید شدن به دستور زبان خاص هست. Nana باعث می شه کد شما سرراست و قابل خواندن بشه. مثلا پاسخ دادن به یک رویداد یا event  

```c++
#include <nana/gui/wvl.hpp>
#include <nana/gui/widgets/button.hpp>

void clicked(const nana::gui::eventinfo&)
{
    //When the window corresponds to fm is clicked,
    //this function will be “called”.
}

int main()
{
    using namespace nana::gui;
    form fm;
    fm.make_event<events::click>(clicked);
    fm.show();
    exec();
}
```

نام تابع کلیک شده یک نام اجباری نیست، هر نامی که می خواهید می توانید بهش بدید. این روش خیلی سرراست تر از روش پیاده سازی یک event handler از وراثت یا همان inheritance در برنامه شماست. در برخی شرایط ، اصلا پارامتر تابع clicked() اهمیتی ندارد. مانند این مثال  

```c++
void clicked() //NO parameter defined.
{
    //When the form is clicked,
    //this function will be “called”.
}
```

بسیار انعطاف پذیره، باعث می شه کد شما ساده بمونه، و این ویژگی می تونه با شیء تابعی هم عملی بشه (function object)

**چه چیزی باعث می شه Nana انقدر قابل انعطاف بشه؟**  
Nana C++ Library حاوی هیچ کامپایلر اضافی برای تفسیر یک دستورزبان خاص نیست. Nana بطور 100% از C++ و تکنیک های الگویی یا همان template استفاده می کنه که باعث می شه این کتابخانه بسیار قدرتمند و قابل انعطاف بشه. Nana بر خلاف کتابخانه های مبتنی بر الگو دیگر که سبب تورم فراوان در کد نویسی می شوند و لازم است که برنامه نویسان با برنامه های مبتنی بر الگو آشنایی داشتنه باشند، نیست و مناسب تازه کاران طراحی شده.
Nana کاملا بر اساس استیل <i dir="ltr" style="direction: ltr;"><code>C++</code></i> پایه گذاری شده و قابل اجرا بر روی Visual C++7.1/GCC 3.4 و نسخه های جدیدتر آنان می باشد. اگر یک برنامه نویس حرفه ای در <i dir="ltr" style="direction: ltr;"><code>C++</code></i> هستید، Nana به شما اجازه استفاده از لاندا یا همان lambda را می دهد، ویژگی جدید در C++11 ، برای پاسخ به رویداد ها. مثل این
```c++
fm.make_event<events::click>([]{
        //When the form is clicked, the object created
        //by this lambda will be “called”.
});

//or

fm.make_event<events::click>([](const eventinfo& ei){
        //When the form is clicked, the object created
        //by this lambda will be “called”, and I can
        //read the parameter.
});
```
علاوه بر این، Nana با اسفتاده از std::bind در C++11 باعث انعطاف پذیری در کد هم می شود.

**Threading (تردینگ)**  
Nana از یک کتابخانه thread-safe بوده و دسترسی به یک ابزارک یا widget بین تردها نرمال شده است. این یک ویژگی عالیه که باعث می شه برنامه نویس پاسخ رویداد رو به ترد دیگر بسادگی ارسال کنه.
```c++
#include <nana/gui/wvl.hpp>
#include <nana/threads/pool.hpp>

void foo()
{
    //This function will be “called” in other
    //thread that is created by thread pool.
}

int main()
{
    using namespace nana::gui;
    using namespace nana::threads;

    pool thrpool;
    form fm;
    fm.make_event<events::click>(pool_push(thrpool, foo));
    fm.make_event<events::click>(pool_push(thrpool, []{
                //A lambda is also allowed.
            }));
    fm.show();
    exec();
}
```

**RAII**  
یک ویژگی مهم که در مثال های بالا نشان داده شد، به محض ایجاد شیء، پنجره وابسته به آن نیز ایجاد می شود و پنجره تا وقتی که show() به شیء فرم فراخوانی نشود مخفی باقی می ماند، و به محض اینکه شیء فرم از بین برود، پنجره وابسته به این نیز بسته می شود. این امر از ایده object life-time در <i dir="ltr" style="direction: ltr;"><code>C++</code></i> پیروی می کند.

**برنامه نویسی چند سکویی**  
Nana C++ Library طراحی شده تا برای برنامه نویسی چند سکویی استفاده شود. اولین نسخه منتشر شده آن تحت ویندوز بود. اکنون این کتابخانه اساسا به لینوکس (X11) پورت شده.

**مهمترین ویژگی: رایگان**  
Nana C++ Library متن باز است. از آن می توانید بطور رایگان هم در مصارف تجاری و هم در مصارف غیر تجاری استفاده کنید.  

برگرفته از [Preliminary Study of Nana C++ Library](http://sourceforge.net/p/nanapro/blog/2012/11/preliminary-study-of-nana-c-library/)

وبسایت کتابخانه: [http://nanapro.org](http://nanapro.org)

