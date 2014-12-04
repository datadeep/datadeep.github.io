---
layout: post
title: "Краткое введение в D3.js"
date: 2014-12-04 20:44:23 +0300
comments: true
categories: [Визуализация]
author: Alexander Anisimov
description: Введение в визуализацию с помощью D3.js
keywords: visualization, D3.js
---

<script src="http://d3js.org/d3.v2.js"></script>
<div>
  <style type="text/css">

    .axis path, .axis line {
      fill: none;
      stroke: #000;
      shape-rendering: crispEdges;
    }
    ul{margin:1em 0 1em 2em;}
    ol{margin:1em 0 1em 2em;}
    
    .container0 .container5{
    	  font-size: x-small;
    }
    


  </style>
</div>

В этой статье мы познакомимся с мощным инструментом визуализации данных – библиотекой D3.js.
Она позволяет представлять ваши данные в различных формах и  добавлять эффектные графики на веб-страницы.

{% img /images/d3.png 768 316 D3.js examples %}

<!-- more -->

Существует много различных инструментов визуализации, некоторые из них имеют долгую историю и плотно прижились среди специалистов. Например, с помощью библиотеки ggplot2 для языка R можно строить достаточно сложные графики всего несколькими строчками кода, а ученые, использующие язык Python, создают графики при помощи библиотеки matplotlib.

Но увеличение количества информации, развитие Интернета и рост графических возможностей современных программ создают новые ожидания от визуального представления данных:

* **Интерактивность.** Интерактивный график может демонстрировать изменения в зависимости от настройки параметров, сравнивать показатели для определенной пользователем выборки или вовсе показывать изменения данных в реальном времени. Такая визуализация не только предъявляет результат, но и позволяет зрителю анализировать данные самостоятельно;
* **Возможность публикации в Интернете.** Визуализация служит не только для анализа данных, это еще и лучший способ поделиться своими выводами с миром. Выложить в сеть статичную картинку нетрудно, но интерактивная визуализация должна быть доступна без установки дополнительных приложений, то есть исполняться прямо в браузере;
* **Доступность данных.** В случае со статичной картинкой, вы не можете получить доступ к исходным данным, чтобы что-то уточнить или проверить. Визуализация, создаваемая в вашем браузере на основе массива данных, не лишает вас такой возможности;
* **Информационная эстетика.** Речь здесь идет не о красоте, а об удобстве восприятия. Настройки по умолчанию для шрифтов и цветовых схем в некоторых системах зачастую не самые удачные, а изменить их не всегда легко. К тому же многие старые системы генерируют только растровые изображения, которые невозможно увеличить без потери качества. Выход: использовать векторную графику и графические возможности современных браузеров.

В связи с этими ожиданиями создаются новые инструменты или улучшаются старые. Например, библиотека Bokeh для языка Python использует для визуализации элемент canvas, созданный как контейнер для графики в HTML5, а инструмент Plotly  позволяет экспортировать результаты работы ggplot2, matplotlib и MATLAB в интерактивные веб-графики.

Но стандартом интерактивных веб-визуализаций можно смело назвать JavaScript библиотеку D3.js Майка Бостока. Библиотека создана в 2011 году, родившись из проекта Protovis. Основной принцип работы D3 заключается в создании элементов  веб-страницы на основе загруженных данных. Для этого используются современные стандарты HTML, SVG (векторная графика), CSS. Основные форматы данных - CSV и JSON.  D3 расшифровывается как Data Driven Documents, что вполне описывает философию библиотеки.

Основные преимущества D3:

*  **Прозрачность связи данных и представления.** Визуальные элементы, создаваемые с помощью библиотеки, напрямую связаны с вашими данными. Данные хранятся в атрибутах графических элементов;
* **Гибкость в выборе представления.** В D3 нет заготовок для различного вида графиков (для хорошей визуализации надо написать не один десяток строк кода), но она позволяет создавать графические элементы и изменять их параметры в соответствии с задачей. Именно поэтому она так хороша для создания нестандартных визуализаций;
* **Интерактивность.** D3 позволяет без особых сложностей превратить график в интерактивный интерфейс, который показывает только актуальную для пользователя информацию.

Очень много крутых примеров можно найти на [сайте](http://bost.ocks.org/mike/) создателя библиотеки Майка Бостока.

##Практический пример

Попробуем создать несложный график и познакомиться с принципами работы D3.

Перед началом работы для удобства отладки необходимо [скачать](https://github.com/mbostock/d3/) библиотеку и сохранить ее в директорию проекта. Структура папок при этом будет выглядеть так:

```
project-folder/
               d3/
                  d3.js
                  d3.min.js (optional)
               index.html
```

Папка с библиотекой может иметь и другое имя, например содержать номер актуальной версии. Слово min означает ограниченный функционал для разработчика, но и ее вполне хватит для начала работы. 

Я не буду останавливаться на описании основ веб-технологии, которые вы можете изучить самостоятельно. Для начала достаточно понимать, как работает HTML-документ, для чего нужны стили CSS и  как устроена SVG графика. Весь приведенный в последующих примерах код должен работать в таком шаблоне (в поле src должен быть указан путь к той версии библиотеки, которую вы скачали, или же ссылка на актуальную версию в Интернете "http://d3js.org/d3.v3.min.js"):

``` html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>D3 Page</title>
        <script type="text/javascript" src="d3/d3.js"></script>
    </head>
    <body>
    <div id="container"></div>
        <script type="text/javascript">
            // Your D3 code will go here
        </script>
    </body>
</html>
```

Начнем изучение с простейшего примера – построим вот такую диаграмму рассеяния (_scatterplot_) для небольшого массива данных:



<div id="container0" align="center"></div>

<script type="text/javascript">
        
        var w = 400;
        var h = 200;
        var padding = 40;

        
        var svg = d3.select("#container0")
             	      .append("svg")
          			  .attr("width", w)
          			  .attr("height", h);
        
        var dataset = [
                [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
                [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];

        
        var xScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[0]; })])
                     .range([padding, w-padding]); 
                     
        var yScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([h - padding, padding]);
                     
        var rScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([2, 5]);  
                     
                                
        var xAxis = d3.svg.axis()
                  .scale(xScale)
                  .orient("bottom")
                  .ticks(5);
                  
         var yAxis = d3.svg.axis()
                  .scale(yScale)
                  .orient("left")
                  .ticks(5);                    
                     
                     
         svg.selectAll("circle")
			   .data(dataset)
			   .enter()
			   .append("circle")
			   .attr("cx", function(d) {return xScale(d[0]);})
			   .attr("cy", function(d) {return yScale(d[1]);})
			   .attr("r", function(d) { return rScale(d[1]);})
			   .attr("fill",function(d){if (d[0]>100) {return "teal"} else {return "orange"}});

		
			svg.append("g")
			    .attr("class", "axis")
			    .attr("transform", "translate(0," + (h - padding) + ")")
                .call(xAxis);  
            svg.append("g")
                .attr("class", "axis")
                .attr("transform", "translate(" + padding + ",0)")
                .call(yAxis);                                 
        
</script> 

Но для начала нарисуем круг. Для этого нам надо выполнить несколько простых шагов:

1. Выделить элемент страницы, на который мы хотим добавить графику. В нашем случае мы выделяем элемент, у которого id = "container";
2. Добавить элемент svg и обозначить его параметры: ширину и высоту;
3. Добавить в SVG контейнер наш кружок;
4. Задать параметры кружка: координаты центра, радиус и цвет.

``` javascript
var svg = d3.select("#container")       //1
            .append("svg")              //2
            .attr("width",100)
            .attr("height",100);          			   
svg.append("circle")                    //3
   .attr("cx", 50)                      //4
   .attr("cy", 50)
   .attr("r", 30)
   .attr("fill","orange");
```

Вот что у нас должно получится:

<div id="container1"></div>
<script>


             var svg = d3.select("#container1")
             	      .append("svg")
          			  .attr("width",100)
          			  .attr("height",100);
          			  
          	svg.append("circle")
			   .attr("cx", 50)
			   .attr("cy", 50)
			   .attr("r", 30)
			   .attr("fill","orange");		  
	    
</script>

Свойства нашего нового объекта мы можем менять не только при создании, но и при дальнейшей работе. Для этого есть два способа:

* создать js-переменную и обращаться к ней;
* обращаться к элементам с помощью функции выделения select или selectAll.

В примере ниже мы меняем радиус круга с помощью первого способа (1), а цвет – с помощью второго (2):

``` javascript
var svg = d3.select("#container")
            .append("svg")
            .attr("width",100)
            .attr("height",100);                        
var circle = svg.append("circle")
            .attr("cx", 50)
            .attr("cy", 50)
            .attr("r", 30);
circle.attr("r",50);                    //1
svg.select("circle")                    //2
   .attr("fill","teal");	
```

<div id="container2"></div>
<script>


             var svg = d3.select("#container2")
             	      .append("svg")
          			  .attr("width",100)
          			  .attr("height",100);
          			  
          	var circle=svg.append("circle")
			   .attr("cx", 50)
			   .attr("cy", 50)
			   .attr("r", 30);
			   
			circle.attr("r",50);
			
			svg.selectAll("circle").
			attr("fill","teal");	
			
				  
	    
</script>

Все это прекрасно, но **где же данные?**

Теперь используем массив данных для создания нескольких кругов.
Для этого применяется не вполне очевидная конструкция:

1. мы задаем данные, которые будем визуализировать
2. выделяем несуществующие элементы: selectAll("circle");
3. связываем их с нашим массивом данных: data(dataset);
4. создаем элементы: enter();
5. добавляем на страницу окружности, используя данные.

Механизм добавления окружностей тот же, что и примере выше. Отличие лишь в том, что окружность теперь создается для каждого элемента массива dataset. Атрибуты окружности (положение её центра) в данном случае определяются конкретными данными.

``` javascript
var svg = d3.select("#container")
            .append("svg")
            .attr("width",200)
            .attr("height",200);          			 
var dataset = [
            [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
            [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];                                            //1	
var circle = svg.selectAll("circle")                        //2
              .data(dataset)                                //3
              .enter()                                      //4
              .append("circle")                             //5
              .attr("cx",function(d){return d[0]})
              .attr("cy",function(d){return d[1]})
              .attr("r", 10);
circle.attr("fill","teal");
```

<div id="container3"></div>
<script>
             var svg = d3.select("#container3")
             	      .append("svg")
          			  .attr("width",200)
          			  .attr("height",200);
          			 
        var dataset = [
                [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
                [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];		  
          	var circle=svg.selectAll("circle")
          	.data(dataset)
          	.enter()
          	   .append("circle")
			   .attr("cx",function(d){return d[0]})
			   .attr("cy",function(d){return d[1]})
			   .attr("r", 10);
			   
			circle.attr("fill","teal");
</script>

Мы нарисовали много кружков, но у нас появилось пара проблем.

Во-первых кружков на рисунке явно меньше, чем элементов в нашем массиве. Это произошло из-за того, что размер нашего svg-элемента всего 200x200 пикселей, а значения в массиве dataset выходят за эти границы.

Во-вторых координатная система в svg устроена так, что ось Y направлена сверху вниз, что нас не устраивает, так как не очень удобно для восприятия.

Обе эти проблемы решаются с помощью функции масштабирования: scale. Это отображение, переводящее число из одного интервала в число из другого интервала. Областью значений чаще всего является разрешение картинки в пикселях. На рисунке ниже показан пример работы масштабирования для числовой и временной осей (рисунок взят из [cтатьи](http://www.d3noob.org/2012/12/setting-scales-domains-and-ranges-in.html) с более подробным пояснением).

{% img /images/scale1.png Scale %}

В примере ниже мы используем два таких отображения (для осей X и Y), задавая области определения (domain), области значений (range) и характер функции (linear).

``` javascript
var w = 400;
var h = 200;
var padding = 30; 
var dataset = [
            [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
            [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];
var xScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[0]; })])
                     .range([padding, w-padding*2]); 
var yScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([h - padding, padding]);
var svg = d3.select("#container")
            .append("svg")
            .attr("width",w)
            .attr("height",h);
var circle = svg.selectAll("circle")
                .data(dataset)
                .enter()
                .append("circle")
                .attr("cx",function(d){return xScale(d[0])})
                .attr("cy",function(d){return yScale(d[1])})
                .attr("r", 5)
                .attr("fill","teal");
```

<div id="container4"></div>
<script>
       var w = 400;
        var h = 200;
        var padding = 30;
        
        
        var dataset = [
                [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
                [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];
                
       var xScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[0]; })])
                     .range([padding, w-padding*2]); 
                     
        var yScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([h - padding, padding]);

             var svg = d3.select("#container4")
             	      .append("svg")
          			  .attr("width",w)
          			  .attr("height",h);
          			 
	
              	  
          	var circle=svg.selectAll("circle")
          	.data(dataset)
          	.enter()
          	   .append("circle")
			   .attr("cx",function(d){return xScale(d[0])})
			   .attr("cy",function(d){return yScale(d[1])})
			   .attr("r", 5)
			   .attr("fill","teal");
</script>

Теперь все точки на месте и будут отображаться корректно, даже если мы решим изменить размер контейнера.

Осталось совсем немного: нарисуем оси с помощью функции d3.svg.axis(), добавим еще один масштаб для радиусов окружности и поиграемся с цветом. Чтобы оси были тонкие и красивые, добавим немного CSS. Итоговый код страницы будет выглядеть как-то так:

{% include_code intro_d3_js.html %}

А вот и результат наших трудов:


<div id="container5">
<style>

</style>
</div>
<script type="text/javascript">
        
        var w = 400;
        var h = 200;
        var padding = 40;

        
        var svg = d3.select("#container5")
             	      .append("svg")
          			  .attr("width", w)
          			  .attr("height", h);
        
        var dataset = [
                [50, 20], [480, 90], [250, 50], [100, 133], [330, 95],
                [410, 12], [475, 44], [25, 167], [185, 21], [120, 88],[600, 150]
              ];

        
        var xScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[0]; })])
                     .range([padding, w-padding]); 
                     
        var yScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([h - padding, padding]);
                     
        var rScale = d3.scale.linear()
                     .domain([0, d3.max(dataset, function(d) { return d[1]; })])
                     .range([2, 5]);  
                     
                                
        var xAxis = d3.svg.axis()
                  .scale(xScale)
                  .orient("bottom")
                  .ticks(5);
                  
         var yAxis = d3.svg.axis()
                  .scale(yScale)
                  .orient("left")
                  .ticks(5);                    
                     
                     
         svg.selectAll("circle")
			   .data(dataset)
			   .enter()
			   .append("circle")
			   .attr("cx", function(d) {return xScale(d[0]);})
			   .attr("cy", function(d) {return yScale(d[1]);})
			   .attr("r", function(d) { return rScale(d[1]);})
			   .attr("fill",function(d){if (d[0]>100) {return "teal"} else {return "orange"}});

		
			svg.append("g")
			    .attr("class", "axis")
			    .attr("transform", "translate(0," + (h - padding) + ")")
                .call(xAxis);  
            svg.append("g")
                .attr("class", "axis")
                .attr("transform", "translate(" + padding + ",0)")
                .call(yAxis);                                 
        
</script>

##Источники для самостоятельного изучения

В этой статье мы очень поверхностно ознакомились с самыми основными принципами рисования графики с помощью D3.js. Для подробного изучения стоит прочитать несколько книг.

Начинать стоит с книги **Interactive Data Visualization**. Лучше использовать ее [бесплатную электронную версию](http://chimera.labs.oreilly.com/books/1230000000345/), потому что там более актуальный код и есть интерактивные примеры.

Для дальнейшего углубления подойдет **Data Visualization with D3.js Cookbook**. В ней хорошо и подробно описаны фундаментальные принципы работы библиотеки.

{% img /images/bookcover1.jpg 272 336 Interactive Data Visualization %} {% img /images/bookcover2.png 272 336 Data Visualization with D3.js Cookbook %}

Помимо книг, есть несколько хороших источников знаний по теме в Интернете:

* [сайт Майка Бостока](http://bost.ocks.org/mike/) --- автора библиотеки. Здесь много хороших примеров и просто интересных статей;
* [wiki проекта на GitHub](https://github.com/mbostock/d3/wiki) --- наиболее полное описание всех функций;
* [Learn how to make Data Visualizations with D3.js](https://www.dashingd3js.com/) --- пошаговое руководство для начинающих;
* [Intro to D3.js](http://square.github.io/intro-to-d3/) --- еще одно;
* [D3.js Tips & Tricks](http://www.d3noob.org/) --- здесь встречаются полезные советы и объяснения тонких мест.

Единственным полезным источником на русском (помимо нашего блога) являются материалы «Лаборатории данных»:

* [«Мыслим связками»](http://d3-js.ru/mike/join/) --- перевод статьи Майка Бостока. Очень полезная статья для понимания связи между данными и представлением в D3;
* [Введение в D3](http://habrahabr.ru/company/datalaboratory/blog/217905/) на Хабре;
* [Онлайн-курс «Визуализация. Основы»](http://brainwashing.pro/dataviz-online) включает в себя раздел про D3.

