# grid
它将网页划分成一个个网格，可以任意组合不同的网格，做出各种各样的布局。以前，只能通过复杂的 CSS 框架达到的效果. 
demo:
```
.container {
    display: grid;
    width: 150px;
    grid-template-columns: 60px 60px;
    grid-template-rows: 30px 90px;
    grid-auto-columns: 60px;
}
.item-a { 
    grid-column: 1 / 2;
    grid-row: 2 / 3;
}
.item-b { 
    /* 容器水平只有2个格子，但这里设定的是第3个，隐式网格创建 */
    grid-column: 3 / 4;
    grid-row: 2 / 3; 
    background-color: rgba(255,255,0, .5);
}
```
比如经典的两栏布局  
```
.wrapper {
  display: grid;
  grid-template-columns: 300px 30%;
}
```
它的构成和flex布局相似 分为容器和item  
[示例代码](https://codesandbox.io/s/griddemo-7nt55)
# 容器
容器里水平区域成为“行”，垂直区域称为“列”，行和列交叉的部分称为“单元格”。
![如图](https://www.wangbase.com/blogimg/asset/201903/1_bg2019032502.png)

划分网格的线，称为"网格线"，它的作用是用来指定网格布局中item所在位置。
![网格线](https://www.wangbase.com/blogimg/asset/201903/1_bg2019032503.png)

要让普通元素称为grid 容器，需要指容器 `display: grid;`

## 容器属性
1. grid-template-columns/grid-template-rows  (**简写grid-template**)
   **grid-template-columns**属性定义每一列的列宽，**grid-template-rows**属性定义每一行的行高。
   ```
   .container {
      display: grid;
      grid-template-columns: 33.33% 33.33% 33.33%; // 可以是1fr/百分比/具体的大小/auto()
      grid-template-rows: 33.33% 33.33% 33.33%;
    }
   ```  
   里面可以使用**repeat** 函数: 基于40px创建栅格，要是我们布局宽度960px，如果一个个些要写24次，有了repeat就方便多了
   ```
   .container {
      display: grid;
      grid-template-columns: repeat(24, 40px);
      grid-template-rows: repeat(3, 33.33%); 
    }
   ```  
   和上面代码等效  
   **fr**: 表示比例关系，网格剩余空间比例单位  
   ```
   grid-template-columns: repeat(2, 100px 20px 80px); // 一共6列， 1、4列100px宽，2、5列 20px宽，3、6列 80px宽
   ```
   网格线命名：在每行（每列）之间用中括号给网格线命名  
   ```
   .container {
      display: grid;
      grid-template-columns: [cloumn-start-line] 100px [第1根纵线结束 第2根纵线开始] 100px [第2根纵线结束 第3根纵线开始] auto [cloumn-end-line];
      grid-template-rows: [r1] 100px [r2] 100px [r3] auto [r4];
    }
   ```

3. grid-template-areas  
  在grid容器中划分区域
  ```
  .container {
    display: grid;
    grid-template-columns: 100px 100px 100px;
    grid-template-rows: 100px 100px 100px;
    grid-template-areas: "header header header"
                     "main main sidebar"
                     "footer footer footer";
  }
  ```
5. grid-grap(grid-row-gap/grid-cloumn-gap) //记住手写“十”字，先一横后一竖  
  设置行间距（列间距） 
8. justify-items: stretch | start | end | center;  
  设置单元格内容的水平位置（左中右）
  stretch  
  默认值，拉伸。表现为水平填充。  
  start  
  表现为网格水平尺寸收缩为内容大小，同时沿着网格线左侧对齐显示（假设文档流方向没有变）。  
  ![justify-item: start](https://www.wangbase.com/blogimg/asset/201903/bg2019032516.png)  
  end  
  表现为网格水平尺寸收缩为内容大小，同时沿着网格线右侧对齐显示（假设文档流方向没有变）。  
  center  
  表现为网格水平尺寸收缩为内容大小，同时在当前网格区域内部水平居中对齐显示（假设文档流方向没有变）。  
9. align-items: stretch | start | end | center;   
  指定了网格元素的垂直呈现方式  
  stretch  
  默认值，拉伸。表现为垂直填充。   
  ![align-items: start](https://www.wangbase.com/blogimg/asset/201903/bg2019032517.png)
  start  
  表现为网格垂直尺寸收缩为内容大小，同时沿着上网格线对齐显示。  
  end  
  表现为网格垂直尺寸收缩为内容大小，同时沿着下网格线对齐显示。  
  center  
  表现为网格垂直尺寸收缩为内容大小，同时在当前网格区域内部垂直居中对齐显示。  
10. place-items:<align-items> / <justify-items>; 上面两个属性的缩写    
11. justify-content  && align-content  
  指定了网格元素的水平分布方式。此属性仅在网格总宽度小于grid容器宽度时候有效果  
  ```
  .container {
    justify-content: start | end | center | stretch | space-around | space-between | space-evenly;
    align-content: start | end | center | stretch | space-around | space-between | space-evenly;  
  }
  ```  
  `justify-content: start`:   
  ![justify-content: start](https://www.wangbase.com/blogimg/asset/201903/bg2019032519.png)  
  `justify-content: center`:    
  ![justify-content: center](https://www.wangbase.com/blogimg/asset/201903/bg2019032520.png)  
  `justify-content: stretch `: 项目大小没有指定时，拉伸占据整个网格容器。     
  ![justify-content: stretch](https://www.wangbase.com/blogimg/asset/201903/bg2019032521.png)  
  `justify-content: space-around `: 每个项目两侧的间隔相等。所以，项目之间的间隔比项目与容器边框的间隔大一倍。     
  ![justify-content: space-around](https://www.wangbase.com/blogimg/asset/201903/bg2019032522.png)  
  `justify-content: space-evenly `: 项目与项目的间隔相等，项目与容器边框之间也是同样长度的间隔。     
  ![justify-content: space-evenly](https://www.wangbase.com/blogimg/asset/201903/bg2019032524.png)  

13. place-content属性是align-content属性和justify-content属性的合并简写形式。  
14. grid-auto-columns/grid-auto-rows  
  指定任何自动生成的网格轨道（也称为隐式网格轨道）的大小。 当网格项目多于网格中的单元格或网格项目放置在显式网格之外时，将创建隐式轨道。
16. grid-auto-flow  
  划分网格以后，容器的子元素会按照顺序，自动放置在每一个网格。默认的放置顺序是"先行后列"，即先填满第一行，再开始放入第二行，即下图数字的顺序。  
17. grid
## 项目属性
1. grid-column-start/grid-column-end grid-row-start/grid-row-end
   表示grid子项所占据的区域的起始和终止位置，包括水平方向和垂直方向。 
   ```
   .item {
        grid-column-start: <number> | <name> | span <number> | span <name> | auto
        grid-column-end: <number> | <name> | span <number> | span <name> | auto
        grid-row-start: <number> | <name> | span <number> | span <name> | auto
        grid-row-end: <number> | <name> | span <number> | span <name> | auto
   }
   ```
   <number>  
   起止与第几条网格线。  
   <name>  
   自定义的网格线的名称。  
   span <number>  
   表示当前网格会自动跨越指定的网格数量。  
   span <name>  
   表示当前网格会自动扩展，直到命中指定的网格线名称。对于命名的网格线，有span和没有span没有区别（包括多个同名网格线）。但是，对于数值网格线，则可以看    出差异，有span则表示跨越的个数，而非网格线的序号。    
   auto  
   全自动，包括定位，跨度等。  
   ```  
   .container {
        grid-template-columns: [第一根纵线] 80px [纵线2] auto [纵线3] 100px [最后的结束线];
        grid-template-rows: [第一行开始] 25% [第一行结束] 100px [行3] auto [行末];
    }
    .item-a {
        grid-column-start: 2;
        grid-column-end: 纵线3;
        grid-row-start: 第一行开始;
        grid-row-end: 3;
    }
   ```  
5. grid-column grid-column-start/grid-column-end 的缩写  
6. grid-row grid-row-start/grid-row-end 的缩写
7. grid-area  
   grid-area表示当前网格所占用的区域
   ```
   .item {
        grid-area: <name> | <row-start> / <column-start> / <row-end> / <column-end>;
    }
   ```
8. justify-self 项目上水平方向的对齐方式
9. align-self 项目上垂直方向上的对齐方式
10. place-self justify-self 和 align-self 的简写
## 参考
[CSS Grid 网格布局教程](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)

[最强大的 CSS 布局 —— Grid 布局](https://juejin.cn/post/6854573220306255880#heading-22)
