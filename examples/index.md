# diamond

----

钻石 Demo

 * <a href="./demo.html">更多demo</a>

----



## 钻石数据

|carat|cut|color|clarity|depth|table|price|x|y|z|
|------------|-----|-----|----|---|-----|---|---|---|--|
|1.01|Premium|I|SI2|61.5|62|3360|6.41|6.37|3.93|

<img src="http://gtms02.alicdn.com/tps/i2/TB1ug0XHFXXXXXbXFXXwbNYRVXX-1252-1080.png" width="800" height="600">

## 命令

### 基本点图
* chart.point().position('carat*price');
* chart.point().position('carat*price').color('color');
* chart.point().position('carat*price').color('color').shape('clarity');

### 分面

* chart.facet(['cut','color']);chart.point().position('carat*price');
* chart.facet(['cut']);
* chart.facet([,'color']);
* chart.facet([]);

### 分布
* chart.point().position('clarity').color('clarity');
* chart.pointStack().position('clarity').color('clarity').size(0.4);
* chart.pointJitter().position('clarity').color('clarity');
* chart.pointDodge().position('clarity').color('cut').size(1.2)
* chart.pointJitter().position('cut*depth').color('cut')

### 柱状图分布
* chart.interval().position(Stat.summary.count('clarity')).color('clarity');
* chart.interval().position(Stat.summary.count(Stat.bin.rect('depth'))).color('red');
* chart.intervalStack().position(Stat.summary.count(Stat.bin.rect('depth'))).color('cut');

### 分布密度
* chart.intervalStack().position(Stat.summary.percent(Stat.summary.count('cut')));
* chart.interval().position(Stat.summary.percent(Stat.summary.count(Stat.bin.rect('depth')))).color('red');
* chart.line().position(Stat.summary.percent(Stat.summary.count(Stat.bin.rect('depth')))).color('red');

### 柱状层叠/百分比

* chart.intervalStack().position(Stat.summary.percent(Stat.summary.count('clarity'))).color('cut');
* chart.intervalStack().position(Stat.summary.percent(Stat.summary.mean('clarity*price'))).color('cut');

### 分析

* chart.interval().position(Stat.summary.mean('cut*price')).color('cut');
* chart.interval().position(Stat.summary.mean('clarity*price')).color('clarity');
* chart.interval().position(Stat.summary.mean('color*price')).color('color');


* chart.pointStack().position('clarity').color('clarity').size(0.4);
* chart.pointJitter().position('clarity').color('clarity');
* chart.pointDodge().position('clarity').color('cut').size(1.2)

### 饼图(theta坐标系,rho坐标系)

* chart.interval().position(Stat.summary.proportion()).color('cut');
* chart.intervalStack().position(Stat.summary.proportion()).color('cut');

* chart.intervalStack().position(Stat.summary.percent(Stat.summary.count())).color('cut');
* chart.intervalStack().position(Stat.summary.percent(Stat.summary.mean('price'))).color('cut');
#### 环图

* chart.coord('theta',{inner: 0.4});
* chart.intervalStack().position(Stat.summary.proportion()).color('cut');

### 其他图

* chart.line().position(Stat.summary.mean('cut*price'));
* chart.area().position(Stat.summary.mean('cut*price'));


### 热力图

* chart.polygon().position(Stat.bin.rect('carat*price',0.01)).color(Stat.summary.count(),'lightness');
* chart.polygon().position(Stat.bin.rect('cut*carat',0.01)).color(Stat.summary.count(),'lightness');
* chart.polygon().position('cut*color').color(Stat.summary.count(),'lightness');


### reflect
* chart.coord('rect').reflect();
* chart.coord('rect').reflect('y');

### scale

* chart.coord('rect').scale(1,-1);
* chart.coord('rect').scale(0.5,0.5);

### rotate

* chart.coord('rect').rotate(60);

### 多坐标轴

* chart.interval().position(Stat.summary.mean('cut*price')).color('cut');
* chart.line().position(Stat.summary.count('cut'))

## 图形类型

### line 

* chart.line().position(Stat.summary.mean('cut*price')).color('color');
* chart.line().position(Stat.summary.mean('cut*price')).shape('smooth')
* chart.line().position(Stat.summary.mean('cut*price')).shape('dot')
* chart.line().position(Stat.summary.mean('cut*price')).shape('dotSmooth')
* chart.line().position(Stat.summary.mean('cut*price')).shape('color');


### area 

* chart.area().position(Stat.summary.mean('cut*price')).color('color')
* chart.area().position(Stat.summary.mean('cut*price')).shape('smooth')
* chart.area().position(Stat.summary.mean('cut*price')).shape('line')
* chart.area().position(Stat.summary.mean('cut*price')).shape('dotLine')
* chart.area().position(Stat.summary.mean('cut*price')).shape('dotSmoothLine')
* chart.area().position(Stat.summary.mean('cut*price')).shape('color');

### interval

* chart.interval().position(Stat.summary.mean('cut*price')).color('cut')
* chart.intervalStack().position(Stat.summary.mean('cut*price')).color('color')
* chart.intervalDodge().position(Stat.summary.mean('cut*price')).color('color')
* chart.interval().position(Stat.summary.mean('cut*price')).color('cut').shape('hollowRect');
* chart.interval().position(Stat.summary.mean('cut*price')).color('cut').shape('line');
* chart.interval().position(Stat.summary.mean('cut*price')).color('cut').shape('tick');
* chart.interval().position(Stat.summary.range('cut*price')).color('cut')


<div>
  <textarea id="txt" cols="80"></textarea>
  <select id="coord">
    <option value="cartesian">cartesian</option>
    <option value="polar">polar</option>
    <option value="theta">theta</option>
    <option value="rho">rho</option>
    <option value="plus">plus</option>

  </select>
  <button id="btn">执行</button>
</div>
<div id="canvas2"></div>
<script src="./js/jquery.min.js?nowrap"></script>
<script src="./js/index.js?nowrap"></script>

````javascript
var g2 = window.G2;
  $.getJSON('./diamond.json?nowrap',function(data){

    var Stat = g2.Stat;
    var select = document.getElementById('sel');
    var chart = new g2.Chart({
      id : 'canvas2',
      width : 800,
      height : 500,
      plotCfg : {
        margin : [50,100,30,50]
      }
    });
  
   // chart.tooltip(false);
    chart.col('clarity',{
      type: 'cat',
      values:['IF','VVS1','VVS2','VS1','VS2','SI1','SI2','I1']
    });

    chart.col('cut',{
      type: 'cat',
      values:['Ideal','Premium','Very-Good','Good','Fair']
    });

    chart.col('color',{
      type: 'cat',
      values:['D','E','F','G','H','I','J']
    });
    
    
    chart.source(data);
    chart.point()
         .position('carat*price')
         .color('clarity');
    chart.render();
    
    $('#btn').on('click',function(){
      chart.clear();
      var cmd = $('#txt').val();
      exec(cmd);
      chart.render();
    });

    $('#coord').on('change',function(){
      var coord = $(this).val();
      chart.coord(coord);
      $('#btn').trigger('click');
    });

    function exec(cmd){
      return eval(cmd);
    }

  });
````