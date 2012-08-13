---
layout: home
title: CSE 2122
---

<div class="left" style="margin-top: -10px;">

<div id="cse" style="width: 100%;">Loading</div>
<script src="http://www.google.com/jsapi" type="text/javascript"></script>
<script type="text/javascript"> 
  google.load('search', '1', {language : 'en'});
  google.setOnLoadCallback(function() {
    var customSearchOptions = {};
    var googleAnalyticsOptions = {};
    googleAnalyticsOptions['queryParameter'] = 'q';
    googleAnalyticsOptions['categoryParameter'] = '';
    customSearchOptions['googleAnalyticsOptions'] = googleAnalyticsOptions;
  
    var customSearchControl = new google.search.CustomSearchControl(
      '007138713826802658404:o_1kwmfq9k4', customSearchOptions);
    customSearchControl.setResultSetSize(google.search.Search.SMALL_RESULTSET);
    customSearchControl.draw('cse');
  }, true);
</script>

* [Syllabus](lecture/syllabus)
* [Course calendar](lecture/calendar)
