---
published: "2024-04-01T13:12:06+03:30"
title: چقدر از سال رفته؟ توییت کنید :))
Description : "هر روز که این صفحه رو باز کنید میشه ببینید و توییت کنید چقدر از سال رفته!"
layout: post
tags: [جاوااسکریپت, twitter, تاریخ-جلالی,  برنامه_نویسی, Hugo]
tweet: https://x.com/mer30hamid/status/1774741022397935866
---


یه اسکریپت نوشتم داخل پست هوگو نشون بده چقدر از سال رفته، آخرشم یه لینک برای توییت زدنش تولید کنه!

خروجیش این میشه:

---
<script>
function isLeapPersian(year) {
    var persianYear = year + 2346;
    var persianYearMinusOne = year - 1 + 2346;
    var daysCurrentYear = Math.ceil(persianYear * 365.24219879);
    var daysPreviousYear = Math.ceil(persianYearMinusOne * 365.24219879);
    return daysCurrentYear - daysPreviousYear === 366;
}
const numOfblocks = 30;
let currentYear = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { year: 'numeric' }));
let allCurrentYearDays = isLeapPersian(currentYear) ? 366 : 365;
let nthDay = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { day: 'numeric' }));
let passedMonth = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { month: 'numeric' })) - 1;
let nthDayOfYear = passedMonth <= 6 ? (passedMonth * 31) + nthDay : (6 * 31) + ((passedMonth - 6) * 30) + nthDay
let filledBlocks = Math.round((nthDayOfYear * numOfblocks) / allCurrentYearDays);
const formatter = new Intl.NumberFormat('en-US', { style: 'percent' });
let percentOfYearPassed = formatter.format(nthDayOfYear/allCurrentYearDays);
let progressTitle = `${nthDayOfYear} روز از سال رفت :\)`;
let progressBody = percentOfYearPassed + " " + ('█').repeat(filledBlocks) + ('░').repeat(numOfblocks-filledBlocks);
document.write(progressTitle + "<br><div dir=ltr>" + progressBody + "<\/div>");
let tweetText = progressTitle + "\n" + progressBody;
document.write(
    '<a class="tweet-button" href="https://x.com/intent/tweet?text=' +
    encodeURIComponent(tweetText) +
    '" target="_blank">👈توییت کنید!👉</a>');
</script>
---
هر روز که این پست رو باز کنید آپدیت می شه پس بوکمارک این صفحه یادتون نره 😅



و اما اسکریپت بدون وابستگی، فقط جاوااسکریپته هر جایی خواستین میشه استفاده کنید:

```javascript
function isLeapPersian(year) {
    var persianYear = year + 2346;
    var persianYearMinusOne = year - 1 + 2346;
    var daysCurrentYear = Math.ceil(persianYear * 365.24219879);
    var daysPreviousYear = Math.ceil(persianYearMinusOne * 365.24219879);
    return daysCurrentYear - daysPreviousYear === 366;
}
const numOfblocks = 30;
let currentYear = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { year: 'numeric' }));
let allCurrentYearDays = isLeapPersian(currentYear) ? 366 : 365;
let nthDay = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { day: 'numeric' }));
let passedMonth = parseInt(new Date().toLocaleDateString('fa-IR-u-nu-latn', { month: 'numeric' })) - 1;
let nthDayOfYear = passedMonth <= 6 ? (passedMonth * 31) + nthDay : (6 * 31) + ((passedMonth - 6) * 30) + nthDay
let filledBlocks = Math.round((nthDayOfYear * numOfblocks) / allCurrentYearDays);
const formatter = new Intl.NumberFormat('en-US', { style: 'percent' });
let percentOfYearPassed = formatter.format(nthDayOfYear/allCurrentYearDays);
let progressTitle = `${nthDayOfYear} روز از سال رفت :\)`;
let progressBody = percentOfYearPassed + " " + ('█').repeat(filledBlocks) + ('░').repeat(numOfblocks-filledBlocks);
document.write(progressTitle + "<br><div dir=ltr>" + progressBody + "<\/div>");
let tweetText = progressTitle + "\n" + progressBody;
document.write(
    '<a class="tweet-button" href="https://x.com/intent/tweet?text=' +
    encodeURIComponent(tweetText) +
    '" target="_blank">👈توییت کنید!👉</a>');
```