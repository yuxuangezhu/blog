---
title: 简单的js日历实现
id: 216
categories:
  - 技术点滴
date: 2016-08-26 12:07:46
tags:
---

一个简单的js日历，支持展示当月以及任意月份展示
<!--more-->
``` html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>日历</title>
<script type="text/javascript" src="http://lib.snowos.cn/jquery.js"></script>
<script type="text/javascript">
$(document).ready(function() {
  $('body').append('<div class="dateMap popover bottom">'
    +'<div class="arrow"></div>'
    +'<h3 class="popover-title">'
      +'<select class="time_year"></select>'
      +'<span>年</span>'
      +'<select class="time_month"></select>'
      +'<span>月</span>'
      +'<input class="time_hour" type="time">'
    +'</h3>'
    +'<div class="popover-content">'
    +'<div class="month-box">'
      +'<ul class="week-title"></ul>'
      +'<ul class="day-box"></ul>'
    +'</div>' 
  +'</div>'
  +'</div>');
  var bigMonth = [1, 3, 5, 7, 8, 10, 12];
  var weekMap = ['日','一','二','三','四','五','六'];
  var time = new Date();
  var year = time.getFullYear();
  var month = time.getMonth() + 1;
  var today = time.getDate();
  var hour = time.getHours();
  if (parseInt(hour) < 10) {
    hour = "0" + hour;
  } 
  var min = time.getMinutes();
  if (parseInt(min) < 10) {
    min = "0" + min;
  } 
  $('.time_hour').val(hour +":" + min);
  weekMap.forEach(function(v) {
    $('.week-title').append('<li>'+ v +'</li>');
  })
  for(var i = 2000; i < 2050; i++) {
    if (i == year) {
      $('.time_year').append('<option value="'+ i +'" selected="true">'+ i +'</option>')
    } else {
      $('.time_year').append('<option value="'+ i +'">'+ i +'</option>');
    }
  }
  for(var j = 0; j < 12; j++) {
    if (j == month -1) {
      $('.time_month').append('<option value="'+ (j + 1) +'" selected="true">'+ (j + 1) +'</option>')
    } else {
      $('.time_month').append('<option value="'+ (j + 1) +'">'+ (j + 1) +'</option>');
    }
  }
  dataMap(year, month);
  function dataMap(year, month) {
    // debugger
    $('.day-box').empty();
    var daysNum = 30;
    var firstDay = new Date(year + '-' + month + '-1');
    var firstWeek = firstDay.getDay();
    if (bigMonth.indexOf(month) != -1) {
      daysNum = 31;
    }
    if (month == 2) {
      daysNum = (year%400 == 0 ? true : ((year%4 == 0 &amp;&amp; year%100 != 0) ? true : false)) ? 29 : 28;
    }
    daysNum = daysNum + firstWeek;

    for(var k = 0; k < daysNum; k++) {
      if (k >= firstWeek) {
        $('.day-box').append('<li>' + (k - firstWeek + 1) + '</li>');
      } else {
        $('.day-box').append('<li></li>')
      }
    }
  }
  $('.time_month').on('change', function(e) {
    e.stopPropagation();
    dataMap($('.time_year').val(), $('.time_month').val());
  });
  $('.time_year').on('change', function(e) {
    e.stopPropagation();
    dataMap($('.time_year').val(), $('.time_month').val());
  });
})
</script>
<style type="text/css">
.month-box {
  width: 273px;
  height: 140px;
  border: 1px solid #000;
}
.week-title {
  padding: 0px;
  margin: 0px;
  height: 20px;
  border-bottom: 1px solid #000;
}
.week-title li {
  float: left;
  list-style-type: none;
  height: 20px;
  width: 39px;
  text-align: center;
}
.day-box {
  padding: 0px;
  margin: 0px;
}
.day-box li {
  float: left;
  list-style-type: none;
  height: 20px;
  width: 39px;
  text-align: center;
}
</style>
</head>
<body>   
</body>
</html>
``` 