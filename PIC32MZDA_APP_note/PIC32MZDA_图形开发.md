
1.PIC32MZ支持DPI直接驱动图形显示器，但目前市面上更多的LCD屏是带GRAM的，也就是说自己LCD模块上自带液晶驱动，这样的话，需要如下几部操作，以确保PIC32MZ能在硬件上连接正确。

```
a.需要连接SPI接口到LCD驱动器的SPI,通过这个接口初始化LCD的图像数据接口工作模式为DPI接口模式。
如何确定SPI口的连接
说明：SPI接口的SCK外设无法重映射，SDI, SDO外设可以重映射。（SCK在封装引脚图中有标识，而SDI,SDO没有）
1.确认SCK对应的是SPI外设的第几个SCK。
2.寻找可支持的SDI,SDO的可映射引脚。
```
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_001.jpg)

```
如上图所示
1.确认使用SPI外设的第几个SCK,如图这里是SCK4.
2.确认SDI可映射引脚
    参考PIZ32MZdatasheet 12.4 peripheral pin select(PPS)章节的 Table 12-2
    因为第1部选择的是SCK4，那么就应该选择SPI4的相关映射，比如这里需要选择SDI4.查找SCK4引脚附件可映射的引脚
    比如这里的PD11. （把这里的0011 = RPD11 的值0011告诉软件工程师，以便相关寄存器选择，
    当然，如果用了HARMONY的话，底层驱动会自己选择）。
```
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_002.jpg)

```
3.确认SDO可映射引脚
同2的选择，这里应该找对应SDO4的外设，寻找离SCK4最近的可选映射引脚，PD0是可选的。
（把这里的0011 = RPD11 的值0011告诉软件工程师，以便相关寄存器选择，当然，如果用了  HARMONY的话，底层驱动会自己选择）。
```
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_003.jpg)

```
b.需要保证PIC32MZ的DPI接口与LCD的DPI接口连接正确。
这部分内容，请参考《Low Cost Controllerless (LCC) Graphics PICtail》--MICROCHIP 官网可以下载。同时还需要查阅实际LCD液晶的驱动说明，及接口说明。
1.核对LCC Graphics PICtail 对外接口的说明
```
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_004.jpg)

```
1.VSYNC & HSYNC
这两个脚可以使用普通GPIO。

2.并行数据信号，可支持18位 RGB 666， 24位 RGB display 888, PIC32MZ实际只有16根PMD信号线
所有实际只能支持到RGB565模式，具体硬件连接请参考下图。
```
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_005.jpg)
来自《Low Cost Controllerless (LCC) Graphics PICtail》的Table 2. 上表有误，正确连接应该如下表。
![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_006.jpg)

 RGB565 PMD正确连接方式
 PMP对外接口示意图，这里涉及液晶驱动部分，只要关注PMD<15：0>的数据总线接口。
 ![images](https://github.com/yuchengstudio/PIC32MZDA/blob/master/PIC32MZDA_APP_note/pictures/PIC32MZDA_GRAPHIC_007.jpg)
 
  来自PIC32MZ datasheet PMP章节。
 

 c:确定LCD屏的硬件接口模式选择是否正确（说明：DPI接口不仅要硬件设置，还需要进一步通过软件SPI设置。）
 DPI（Display Pixel Interface）规范中所规定的硬件接口跟DBI规范中并不相同，它不是像DBI规范用Command/Data配置液晶驱动IC的寄存器再进行操作。某种程度上，DPI与DBI的最大差别是DPI的数据线和控制线分离，而DBI是复用的。
 






