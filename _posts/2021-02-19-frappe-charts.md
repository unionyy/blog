---
title: "JavaScript 차트 만들기[Frappe Charts]"
excerpt_separator: "<!--more-->"
layout: single
date: 2021-02-19T10:19:30+09:00
categories:
  - JavaScript
tags:
  - JavaScript
  - Frappe Charts
permalink: /js/frappe-charts/
---
---
[Frappe Charts](https://frappe.io/charts){:target="_blank"}는 SVG 차트를 쉽게 만들게 해주는 오픈소스 모듈입니다. 여러가지 종류의 깔끔한 그래프들을 간편하게 만들 수 있습니다.
<!--more-->

## CDN을 사용하여 설치하지 않고 사용하기
```html
<script src="https://cdn.jsdelivr.net/npm/frappe-charts@1.5.6/dist/frappe-charts.min.iife.js"></script>
```
해당 코드를 웹페이지의 head 태그 안에 삽입하면 CDN을 통해 Frappe Charts를 이용할 수 있습니다. 현재 기준으로는 1.5.6버전이 최신버전입니다.  [GitHub Release](https://github.com/frappe/charts/releases){:target="_blank"}에서 최신버전을 확인하시고 사용하시는 것을 추천합니다.

CDN에 대한 설명은 jQuery 관련 포스트를 참고하세요.
* [jQuery 설치 없이 사용하기[CDN]](/js/jquery-cdn/){:target="_blank"}

## 차트 삽입
차트를 삽입할 위치에 div 태그를 추가하고 id를 설정해줍니다.
```html
<div id="chart"></div>
```
그리고 `frappe.Chart()` 함수를 사용해 차트를 생성해줍니다.
```html
<script>
    const data = {
      labels: [
                '0min',  '1min',  '2min',  '3min',
                '4min',  '5min',  '6min',  '7min',
                '8min',  '9min',  '10min', '11min',
                '12min', '13min', '14min', '15min',
                '16min', '17min', '18min', '19min',
                '20min', '21min', '22min', '23min',
                '24min', '25min', '26min', '27min',
                '28min', '29min', '30min', '31min',
                '32min'
              ],
      datasets: [
        {
            name: "Player 1",
            values: [
              500, 0, 21, 199, 564, 275, 510,
              340, 671, 549, 868, 66, 78, 410,
              701,  971,  231, 646,  811, 1241,  99,
              146,  712,  140, 485,  483,  711, 237,
              410, 1089, 1460, 875, 1171
            ]
        },
        {
            name: "Player 2",
            values: [
              500,    0,  194,  548,  748, 1401,
              149,  734, 1231,  214,  336,  949,
              1681,  133,  364,  601, 1136,  253,
              590,  112,  514,   11,  559,  376,
              1214,   37,  544,  117,  719, 1249,
              1438, 2007,  276
            ]
        }
      ]
    }
            
    const chart = new frappe.Chart("#chart", {
      title: "Current Gold",
      data: data,
      type: 'line', // 'bar', 'line', 'scatter', 'pie', 'percentage'
      height: 250,
      colors: ['#7cd6fd', '#743ee2']
      lineOptions: {
        hideDots: 1, // default: 0
        regionFill: 1, // default: 0
      }
    })
</script>
```
`data` 옵션을 통해 `label`과 `datasets`을 설정해줍니다.

그 외에 여러 옵션들을 수정하면서 원하는 그래프를 만들 수 있습니다.

* [Configuration](https://frappe.io/charts/docs/reference/configuration){:target="_blank"}

![line chart](/assets/post-images/frappe-charts/line.png)

## 그래프 숨기기

Frappe Charts는 아직까지 유저가 원하는 데이터만을 보여주는 기능을 제공하지 않습니다. (제가 찾지 못한 것이라면 알려주세요 ㅠ)

CSS의 display 옵션을 이용해서 각각의 데이터를 보여주거나 없애는 토글 버튼을 만들었습니다.

먼저 default로 모든 그래프를 보이지 않게 설정해주었습니다. (head 태그에 추가)
```html
<style>
    .dataset-units {
        display: none;
    }
</style>
```

`Hide()` 함수를 만들어서 인자로 받은 dataset을 보여주거나 사라지게 했습니다. (head 태그에 추가)
```html
<script>
    function Hide(dataset) {
        if(document.getElementsByClassName(dataset)style.display === 'block') {
            document.getElementsByClassName(dataset)style.display = 'none';
        } else {
            document.getElementsByClassName(dataset)style.display = 'block';
        }
    }
</script>
```

마지막으로 body 태그에 각각의 데이터셋에 해당하는 버튼을 만들어줍니다.

```html
<button onclick="Hide('dataset-0')">Player 1</button>
<button onclick="Hide('dataset-1')">Player 2</button>
<button onclick="Hide('dataset-2')">Player 3</button>
```
![toggle](/assets/post-images/frappe-charts/toggle.png)

## 히트맵, GitHub Contribution Graph

Frappe Charts는 히트맵 그래프 기능을 제공합니다. 이를 이용하면 GitHub의 Contribution Graph와 같은 그래프를 쉽게 만들 수 있습니다.

* [히트맵](https://frappe.io/charts/docs/basic/heatmapn){:target="_blank"}

![hitmap](/assets/post-images/frappe-charts/hitmap.png)

[LoLog.me](https://lolog.me){:target="_blank"} 사이트를 만들 때, 사각형을 하나하나 찍는 코드를 짜서 비슷한 기능을 구현했었습니다. 이 모듈을 일찍 알았더라면 더 쉽게 구현할 수 있었을 듯 합니다...

## 완성된 코드 [GitHub](https://github.com/unionyy/practice/blob/main/graph-javascript/practice.html){:target="_blank"}

```html
<!DOCTYPE html>
<html lang="kr">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Chart!</title>

        <script src="https://cdn.jsdelivr.net/npm/frappe-charts@1.5.6/dist/frappe-charts.min.iife.js"></script>

        <style>
            .dataset-units {
                display: none;
            }
        </style>

        <script>
            function Hide(dataset) {
                if(document.getElementsByClassName(dataset)[0].style.display === 'block') {
                    document.getElementsByClassName(dataset)[0].style.display = 'none';
                } else {
                    document.getElementsByClassName(dataset)[0].style.display = 'block';
                }
            }
        </script>
    </head>
    <body>
        <div id="chart"></div>
        
        <script>
            const data = {
                labels: [
                    '0min', '1min', '2min', '3min',
                    '4min', '5min', '6min', '7min',
                    '8min', '9min', '10min', '11min',
                    '12min', '13min', '14min', '15min',
                    '16min', '17min', '18min', '19min',
                    '20min', '21min', '22min', '23min',
                    '24min', '25min', '26min', '27min',
                    '28min', '29min', '30min', '31min',
                    '32min'
                ],
                datasets: [
                    {
                        name: "Player 1",
                        values: [
                            500, 0, 21, 199, 564, 275, 510,
                            340, 671, 549, 868, 66, 78, 410,
                            701, 971, 231, 646, 811, 1241, 99,
                            146, 712, 140, 485, 483, 711, 237,
                            410, 1089, 1460, 875, 1171
                        ]
                    },
                    {
                        name: "Player 2",
                        values: [
                            500, 0, 194, 548, 748, 1401,
                            149, 734, 1231, 214, 336, 949,
                            1681, 133, 364, 601, 1136, 253,
                            590, 112, 514, 11, 559, 376,
                            1214, 37, 544, 117, 719, 1249,
                            1438, 2007, 276
                        ]
                    },
                    {
                        name: "Player 3",
                        values: [
                            500, 0, 63, 395, 753, 1474,
                            1845, 55, 111, 544, 865, 344,
                            394, 735, 1038, 1432, 40, 458,
                            1057, 276, 399, 770, 1313, 490,
                            904, 22, 215, 543, 257, 797,
                            954, 1536, 1992
                        ]
                    }
                ]
            }

            const chart = new frappe.Chart("#chart", {  // or a DOM element,
                // new Chart() in case of ES6 module with above usage
                title: "Current Gold",
                data: data,
                type: 'line', // or 'bar', 'line', 'scatter', 'pie', 'percentage'
                height: 250,
                //colors: ['#7cd6fd', '#743ee2']
                lineOptions: {
                    hideDots: 1, // default: 0
                    regionFill: 1, // default: 0
                }
            })
        </script>

        <button onclick="Hide('dataset-0')">Player 1</button>
        <button onclick="Hide('dataset-1')">Player 2</button>
        <button onclick="Hide('dataset-2')">Player 3</button>
    </body>
</html>
```

## Reference
* [Frappe Charts - Docs](https://frappe.io/charts/docs){:target="_blank"}
* [jQuery 설치 없이 사용하기[CDN]](/js/jquery-cdn/){:target="_blank"}