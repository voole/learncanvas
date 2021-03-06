## 1.绘制矩形
```js
clearRect(double x, double y, double w, double h);
fillRect(double x, double y, double w, double h);
strokeRect(double x, double y, double w, double h);
```
![绘制矩形](img/draw_rect.png)

## 2.透明填充
> color填充: ctx.fillStyle = rgba(0, 0, 0, .5);

> 设置全局透明: ctx.globalAlpha = .5;

## 3.渐变色

![渐变色](img/gradient.png)

### 3.1 横向线性渐变
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");

var gradient = ctx.createLinearGradient(0,0,c.width, 0);
gradient.addColorStop(0, "#e6e600");
gradient.addColorStop(0.25, "#d1c50b");
gradient.addColorStop(0.5, "#d49500");
gradient.addColorStop(0.75, "#dd8810");
gradient.addColorStop(1, "#ff0000");

ctx.fillStyle = gradient;
ctx.fillRect(0, 0 , c.width, c.height);
```

### 3.2 斜向线性渐变
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");

var gradient = ctx.createLinearGradient(0, 0, c.width, c.height);
gradient.addColorStop(0, "#e6e600");
gradient.addColorStop(0.25, "#d1c50b");
gradient.addColorStop(0.5, "#d49500");
gradient.addColorStop(0.75, "#dd8810");
gradient.addColorStop(1, "#ff0000");

ctx.fillStyle = gradient;
ctx.fillRect(0, 0 , c.width, c.height);
```

### 3.3 放射渐变
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");

var gradient = ctx.createRadialGradient(
    c.width/2, 0, 10,
    c.width/2, c.height, 100
);
gradient.addColorStop(0, "#e6e600");
gradient.addColorStop(0.25, "#d1c50b");
gradient.addColorStop(0.5, "#d49500");
gradient.addColorStop(0.75, "#dd8810");
gradient.addColorStop(1, "#ff0000");

ctx.fillStyle = gradient;
ctx.fillRect(0, 0 , c.width, c.height);
```

## 4.图案填充
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");
var img = new Image();
img.src="img/icon.png";
img.onload = function (e) {
    //repeat, repeat-x, repeat-y, no-repeat
    var pattern = ctx.createPattern(img, "repeat");
    ctx.fillStyle = pattern;
    ctx.fillRect(0,0,c.width,c.height);
}
```

## 5.阴影
![shadow](img/shadow.png)
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");

ctx.shadowColor = "rgba(255, 0, 0, .5)";
ctx.shadowOffsetX = 1;
ctx.shadowOffsetY = 1;
ctx.shadowBlur = 8;

ctx.font = "bold 30pt 微软雅黑";
ctx.fillText("Hello Canvas", c.width/2, c.height/2);
```

## 6.绘制路径
![shadow](img/path.png)
```js
var c = document.querySelector("#canvas");
var ctx = c.getContext("2d");

ctx.strokeStyle = "#f00";
ctx.lineWidth = 2;
ctx.lineJoin = "round";
ctx.fillStyle = "rgba(255,0,0,.5)";

ctx.beginPath();
//x,y,radius,startAngle,endAngle,anticlockwise
ctx.arc(100, 100, 50, 0, Math.PI/2, true);
ctx.stroke();

ctx.beginPath();
ctx.arc(100, 100, 50, 0, Math.PI/2, false);
ctx.lineTo(100, 100);
ctx.closePath();
ctx.stroke();
ctx.fill();

ctx.beginPath();
//x, y, w, h
ctx.rect(200, 100, 200, 100);
ctx.stroke();
ctx.fill();
```
#### 6.1绘制等边N边形
```js
    createNPolygon(300, 300, 100, 4, 60);

    function createNPolygon(centerx, centery, radius, sides, startAngle) {
        var _perAngle = Math.PI*2/sides;
        for(var i=0;i<sides;i++){
            var _angle = _perAngle*i + Math.PI/180*startAngle;
            var _x = Math.cos(_angle)*radius,
                _y = Math.sin(_angle)*radius;
            _x = centerx+_x;
            _y = centery-_y;
            if(i===0) ctx.moveTo(_x, _y);
            else ctx.lineTo(_x, _y)
        }
        ctx.closePath();
        ctx.stroke();
    }
```

## 7.绘制圆弧
![绘制圆弧](img/draw_arc.png)
```js
ctx.arc(500,200,50, 0, Math.PI/2, false);//逆时针

function roundedRect(cornerX, cornerY, width, height, cornerRadius) {
   if (width > 0) context.moveTo(cornerX + cornerRadius, cornerY);
   else           context.moveTo(cornerX - cornerRadius, cornerY);

   context.arcTo(cornerX + width, cornerY,
                 cornerX + width, cornerY + height,
                 cornerRadius);

   context.arcTo(cornerX + width, cornerY + height,
                 cornerX, cornerY + height,
                 cornerRadius);

   context.arcTo(cornerX, cornerY + height,
                 cornerX, cornerY,
                 cornerRadius);

   if (width > 0) {
      context.arcTo(cornerX, cornerY,
                    cornerX + cornerRadius, cornerY,
                    cornerRadius);
   }
   else {
      context.arcTo(cornerX, cornerY,
                    cornerX - cornerRadius, cornerY,
                    cornerRadius);
   }
}

function drawRoundedRect(strokeStyle, fillStyle, cornerX, cornerY, width, height, cornerRadius) {
   context.beginPath();

   roundedRect(cornerX, cornerY, width, height, cornerRadius);

   context.strokeStyle = strokeStyle;
   context.fillStyle = fillStyle;

   context.stroke();
   context.fill();
}
```

## 8.贝萨尔曲线
### 8.1 二次方贝萨尔曲线
```js
ctx.beginPath();
ctx.moveTo(100, 100);
//绘制正余弦曲线效果
//quadraticCurveTo接受四个参数，cpx，cpy, x, y
ctx.quadraticCurveTo(150, 50, 200, 100);
ctx.quadraticCurveTo(250, 150, 300, 100);
ctx.quadraticCurveTo(350, 50, 400, 100);
ctx.quadraticCurveTo(450, 150, 500, 100);
ctx.stroke();
```
```js
//类似于圆角三角形的效果
ctx.beginPath();
ctx.moveTo(950, 500);
ctx.quadraticCurveTo(1000, 500, 975, 457);
ctx.lineTo(775, 110);
ctx.quadraticCurveTo(750, 67, 725, 110);
ctx.lineTo(525, 457);
ctx.quadraticCurveTo(500, 500, 550, 500);
ctx.closePath();
ctx.stroke();
```
### 8.2 三次方贝萨尔曲线
```js
//类似于圆角三角形的效果
ctx.beginPath();
ctx.moveTo(950, 500);
ctx.quadraticCurveTo(1000, 500, 975, 457);
ctx.lineTo(775, 110);
ctx.quadraticCurveTo(750, 67, 725, 110);
ctx.lineTo(525, 457);
ctx.quadraticCurveTo(500, 500, 550, 500);
ctx.closePath();
ctx.stroke();
```

