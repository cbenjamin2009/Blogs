# Zero Based getMonth() Lesson

## A lesson learned in using getMonth() method of the Date prototype
Yesterday I had a project where I was adding a small feature to an internal website to alert the user if they had a date issue when completing a web form for current date. This feature compared local computer date/time to that of a HTML Date type input element entered by the user. If the date they entered was not today then it would give them a warning message about entering data for the wrong date. This was to correct a problem with users inputting incorrect data. 

My approach to this was to do a quick compare of a new Date variable against the value in the input from the user and then trigger an `alert()` to the user if there was a mismatch. 

```javascript
let myDate = new Date()
console.log(myDate)
// Thu Aug 10 2022 12:39:27 GMT-0700 (Pacific Daylight Time)
```

Perfect, we have a variable that is pulling the correct date/time.

HTML elements store the date in YYYY/MM/DD format and getDate pulls in MM/DD/YYYY order so they cannot be directly compared. 

My approach was to reverse the date string to quickly match that of the users. There's several ways to do this but this is the step I used. 
```javascript
  let myDate = new Date();
  let newDate = []
newDate.push(myDate.getFullYear(), '-', myDate.getMonth(), '-', myDate.getDay())
// -> [2022,-,7,-,3]
console.log('newDate.join()' + newDate.join(''))
// -> 2022-7-3
```
Do you see the errors? There were two, the first is the month, and the second is the day. In my haste to create this quick feature I used getDay() instead of getDate() which gave me the wrong calendar date. 

I did not notice the errors and was quite puzzled that the getMonth() was pulling from the month prior but when I made other attempts to extract the date it was pulling month 8. So I went to Twitter and asked the community. 

{% embed https://twitter.com/_ChrisBenjamin/status/1557395578245369857 %}

Several helpful people quickly responded with the answer, which until this time, I had no clue getMonth() pulled zero-based months in all of these years working with dates. Several people referred me to [MDN getMonth](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date/getMonth) which clearly explains what is happening. 

The other error is about the getMonth(). This function extracts the month from the date in a **Zero Based Value**. This means that for the calendar year, the months are numbered 0-11, whereas August is 7 instead of 8. 

> The getMonth() method returns the month in the specified date according to local time, as a zero-based value (where zero indicates the first month of the year). - MDN

In most cases when I'm extracting dates, I'm using `myDate.toLocaleString()` or `myDate.toLocaleDateString()` which mask the output and show the proper month because of locale. 
```javascript
myDate.toLocaleString()
// -> '8/11/2022, 12:54:53 PM'
myDate.toLocaleDateString()
// -> '8/11/2022'
```

## Conclusion
I wanted to share this information with all of you as to help someone else avoid running into this same error in the future. Every time I work with dates I run into some type of issue and this just happened to be yet another lesson to learn in working with dates. 