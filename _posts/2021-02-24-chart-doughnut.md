---
title: "JavaScript 도넛 차트 만들기[Google Charts]"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-24T22:21:30+09:00
categories:
  - JavaScript
tags:
  - JavaScript
  - Google Charts
permalink: /js/charts-donut/
---
---
Google Charts는 예전에 포스팅했던 [Frappe Charts](/js/frappe-charts){:target="_blank"}와 비슷한 기능을 하는 자바스크립트 차트 라이브러리입니다. 사용법도 거의 비슷하고, 구글에서 만든 모듈인지라 문서화가 잘 돼있습니다. [Google Charts](https://developers.google.com/chart){:target="_blank"}
<!--more-->

{% include ad-contents.html %}

## JavaScript 차트 라이브러리

* [Frappe Charts](https://frappe.io/charts){:target="_blank"}

* [Google Charts](https://developers.google.com/chart){:target="_blank"}

* [Charts.js](https://www.chartjs.org/){:target="_blank"}

세 라이브러리 모두 도넛차트를 지원합니다. 원하는 디자인이나 기능을 따져보고 사용할 라이브러리를 선택하시면 됩니다.

## Google Charts 설정 및 파이차트 생성

[Quick Start](https://developers.google.com/chart/interactive/docs/quick_start){:target="_blank"}

```html
<html>
  <head>
    <!--Load the AJAX API-->
    <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <script type="text/javascript">

      // Load the Visualization API and the corechart package.
      google.charts.load('current', {'packages':['corechart']});

      // Set a callback to run when the Google Visualization API is loaded.
      google.charts.setOnLoadCallback(drawChart);

      // Callback that creates and populates a data table,
      // instantiates the pie chart, passes in the data and
      // draws it.
      function drawChart() {

        // Create the data table.
        var data = new google.visualization.DataTable();
        data.addColumn('string', 'Topping');
        data.addColumn('number', 'Slices');
        data.addRows([
          ['Mushrooms', 3],
          ['Onions', 1],
          ['Olives', 1],
          ['Zucchini', 1],
          ['Pepperoni', 2]
        ]);

        // Set chart options
        var options = {'title':'How Much Pizza I Ate Last Night',
                       'width':400,
                       'height':300};

        // Instantiate and draw our chart, passing in some options.
        var chart = new google.visualization.PieChart(document.getElementById('chart_div'));
        chart.draw(data, options);
      }
    </script>
  </head>

  <body>
    <!--Div that will hold the pie chart-->
    <div id="chart_div"></div>
  </body>
</html>
```

{% include ad-contents.html %}

## 파이차트를 도넛차트로

파이차트에 pieHole 옵션을 추가해주면 파이차트의 중앙에 구멍이 생겨 도넛차트가 됩니다.

```javascript
var options = {
      pieHole: 0.41
    }
```

## 커스터마이징
여러가지 옵션을 주면서 차트를 커스터마이징합니다.

* [Configuration Options](https://developers.google.com/chart/interactive/docs/gallery/piechart#configuration-options){:target="_blank"}

```javascript
var options = {
  pieHole: 0.41,  // 도넛차트
  chartArea: {
    /* 차트크기 */
    width: 200,
    height: 200
    },

  /* 전체크기 */
  width: 300,
  height: 300,

  /* 배경색 설정 */
  backgroundColor: 'whitesmoke',

  /* 차트에 레이블 표시 (percentage, value or label)*/
  pieSliceText: 'label',

  /* 꼬리표 제거 */
  legend: 'none'
};
```

{% include ad-contents.html %}

## 도넛차트 중앙에 이미지 추가
CSS의 position 속성(relative, absolute)를 이용합니다. 차트와 이미지를 감싸는 부모 `<div>` 태그에 `position: relative;`, 이미지 태그에 `position: absolute;`의 CSS 설정을 해준 뒤에 위치를 조정하면 됩니다.

```css
#container {
    position: relative;
}

#piechart {
    width: 900px;
    height: 500px;
}

#overlay {
    position: absolute;
    width: 80px;
    height: 80px;
    top: 110px;
    left: 110px;
    border-radius: 40px;
}
```

## 완성된 코드 [GitHub](https://github.com/unionyy/practice/blob/main/javascript-charts/google-donut.html){:target="_blank"}

```html
<!DOCTYPE html>
<html>
    <head>
        <title>차트테스트</title>
        <meta charset="utf-8">
        <script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
    <style>
      #container{
        position: relative;
      }

      #piechart{width: 900px; height: 500px;}

      #overlay {
        position: absolute;
        width: 80px;
        height: 80px;
        top: 110px;
        left: 110px;
        border-radius: 40px;
      }
    </style>
    
    <script type="text/javascript">
      google.charts.load('current', {'packages':['corechart']});
      google.charts.setOnLoadCallback(drawChart);

      function drawChart() {

        var data = google.visualization.arrayToDataTable([
          ['Task', 'Hours per Day'],
          ['Work',     11],
          ['Eat',      2],
          ['Commute',  2],
          ['Watch TV', 2],
          ['Sleep',    7]
        ]);

        var options = {
          pieHole: 0.41,  // 도넛차트
          chartArea: {
            /* 차트크기 */
            width: 200,
            height: 200
          },
          /* 전체크기 */
          width: 300,
          height: 300,

          /* 차트에 레이블 표시 (percentage, value or label)*/
          pieSliceText: 'label',

          /* 꼬리표 제거 */
          legend: 'none'
        };

        var chart = new google.visualization.PieChart(document.getElementById('piechart'));

        chart.draw(data, options);    
      }
    </script>
        
    </head>
    <body>
      <div id="container">
        <div id="piechart"></div>
        <img id="overlay" src="https://ddragon.leagueoflegends.com/cdn/11.4.1/img/champion/Shen.png">
      </div>
    </body>
</html>
```

{% include ad-contents.html %}

## Reference

* [Google Charts](https://developers.google.com/chart){:target="_blank"}

* [CSS position 속성으로 div 위에 div 겹치기](https://heinafantasy.com/62){:target="_blank"}