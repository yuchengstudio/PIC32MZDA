
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


