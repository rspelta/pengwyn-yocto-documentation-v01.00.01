Hardware
========

**Jumper settings**

+--------------------+---------------------------------------+
|                    |            boot sequence              |
+------+------+------+---------+---------+---------+---------+
|  J1  |  J2  |  J3  |   1st   |   2nd   |   3rd   |   4th   |
+======+======+======+=========+=========+=========+=========+
| open | open | open |  UART0  |  SPI0   | XIP MUX2|  MMC0   |
+------+------+------+---------+---------+---------+---------+
| open | open |close |  EMAC1  |  MMC0   | XIP MUX2|  NAND   |
+------+------+------+---------+---------+---------+---------+
| open |close | open |  USB0   |  NAND   |  SPI0   |  MMC0   |
+------+------+------+---------+---------+---------+---------+
| open |close |close | Fast ext|  UART0  |  EMAC1  |reserved |
+------+------+------+---------+---------+---------+---------+
|close | open | open |  NAND   | NANDI2C |  MMC0   |  UART0  |
+------+------+------+---------+---------+---------+---------+
|close | open |close |  MMC0   |  SPI0   |  UART0  |  USB0   |
+------+------+------+---------+---------+---------+---------+
|close |close | open | XIP MUX2|  UART0  |  SPI0   |  MMC0   |
+------+------+------+---------+---------+---------+---------+
|close |close |close | Fast ext|  EMAC1  |  UART0  |reserved |
+------+------+------+---------+---------+---------+---------+


Link to Assembly_

.. _Assembly: http://www.silica.com/fileadmin/02_Products/Productdetails/Silica/Silica_pengwyn-905B_Assembly.pdf


Link to BOM_

.. _BOM: http://www.silica.com/fileadmin/02_Products/Productdetails/Silica/Silica_pengwyn-905B_BOM.pdf


Link to Schematic_

.. _Schematic: http://www.silica.com/fileadmin/02_Products/Productdetails/Silica/Silica_pengwyn-905B_Schematic.pdf
