# imin_printing_sdk
Modifiy Imin printing
PrinterSDK
================

1.Call the printer via SDK
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

iMin SDK provides packaged common printing instructions, so that developers can quickly access iMin printers

`Print Demo source code <https://docs.imin.sg/docs/demo/iMinPrinerSDK.zip>`_

**1ã€Printer initialization**

Functionï¼švoid initPrinter()

Remarksï¼šReset the printer's logic program (for example: layout settings, bold and other style settings), but do not clear the buffer data, so unfinished print jobs will continue after reset.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).initPrinter(IminPrintUtils.PrintConnectType.SPI);

parameterï¼š

        IminPrintUtils.PrintConnectType.USB      --> USB

        IminPrintUtils.PrintConnectType.SPI      --> SPI

        IminPrintUtils.PrintConnectType.Bluetooth      --> Bluetooth

**2ã€Get printer status**

Function by USB(D4&S1)ï¼šint getPrinterStatus(IminPrintUtils.PrintConnectType type)

Function by SPI(M2)ï¼švoid getPrinterStatus(IminPrintUtils.PrintConnectType type, final Callback callback)

return value:

        -1 --> The printer is not connected or powered on

        0 --> The printer is normal

        1 --> The printer is not connected or powered on

        3 --> Print head open

        7 --> No Paper Feed

        8 --> Paper Running Out

        99 --> Other errors

.. code-block:: xml
    :linenos:
    :emphasize-lines: 0

        int status = IminPrintUtils.getInstance(TestPrintActivity.this).getPrinterStatus(IminPrintUtils.PrintConnectType.USB );

        IminPrintUtils.getPrinterStatus(IminPrintUtils.PrintConnectType.SPI, new Callback() {
                                                    @Override
                                                    public <T> void callback(T t) {
                                                        int status = (int) t;
                                                    }
                                                });


**3ã€Print and feed paper**

Functionï¼švoid printAndLineFeed()

Remarksï¼šThe printer runs on 1 lines of paper

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printAndLineFeed();

**4ã€Print blank lines (custom height)**

Functionï¼švoid printAndFeedPaper(int value)

Remarksï¼šThe maximum paper distance is 1016mm (40 inches), if this distance is exceeded, the maximum distance is taken

parameterï¼š

        1. value      -->  heightï¼ˆ0-255ï¼‰.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printAndFeedPaper(100);

**5ã€ Cutter (paper cutting) correlation**

Functionï¼švoid partialCut()

Remarksï¼šequipment support is required

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).partialCut();

**6ã€Set text alignment**

Functionï¼švoid setAlignment(int alignment)

parameterï¼š

        1. alignment  -->  Set text alignment 0 = left / 1 = center / 2 = right / default = 0

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setAlignment(1);

**7ã€Set text size**

Functionï¼švoid setTextSize(int size)

parameterï¼š

        1. size       -->  Set text size default 28

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setTextSize(26);

**8ã€Set font**

Functionï¼švoid setTextTypeface(Typeface typeface)

parameterï¼š

        1. typeface   -->  Set font default Typeface.DEFAULT

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setTextTypeface(Typeface.DEFAULT);

**9ã€Set font style**

Functionï¼švoid setTextStyle(int style)

parameterï¼š

        1. style      -->  Set the font style (bold or italic) NORMAL = 0  BOLD = 1  ITALIC = 2  BOLD_ITALIC = 3

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setTextStyle(1);

**10ã€Set line spacing**

Functionï¼švoid setTextLineSpacing(float space)

parameterï¼š

        1. space      -->  Set line spacing default 1.0f

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setTextLineSpacing(1.0f);

**11ã€Set print width**

Functionï¼švoid setTextWidth(int width)

parameterï¼š

        1. width      -->  Default 80mm printer default effective print width 576

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setTextWidth(576);

**12ã€Print text**

Functionï¼švoid printText(String text)

parameterï¼š

        1. text       -->  Print content; will automatically wrap

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printText("æ‰“å°å†…å®¹");

**13ã€Print text**

Functionï¼švoid printText(String text, int type)

parameterï¼š

        1. text       -->  When the printed content is less than one or more lines, you need to add a line break "\n" at the end of the content to print it immediately, otherwise it will be cached in the buffer.
        2. type       -->  Default value 0;

                                0 = you need to add a line break "\n" at the end of the content to print it immediately, otherwise it will be cached in the buffer.

                                1 = Word wrap

        Note: to change the style of the printed text (such as alignment, font size, bold, etc.),set it before calling the printText method.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printText("æ‰“å°å†…å®¹", 0);

**14ã€Print a row of the table (not support Arabic)**

Functionï¼švoid printColumnsText(String[] colTextArr, int[] colWidthArr, int[] colAlign, int width, int[] size)

parameterï¼š

        1. colTextArr   -->  column text string array
        2. colWidthArr  -->  Array of the width of each column, calculated in English characters, each Chinese character occupies two English characters, each width is greater than 0.
        3. colAlign     -->  alignment: 0 to the left, 1 to the center, and 2 to the right
        4. size         -->  Font size per column string array
        5. width        -->  Print the total width of a line (80mm printing paper = 576)

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printColumnsText(new String[]{"1","iMin","iMin"} ,new String[]{1,2,1} ,new String[]{1,0,2} ,new String[]{26,26,26} ,576);

**15ã€Set barcode width**

Functionï¼švoid setBarCodeWidth(int width)

parameterï¼š

        1. width      -->  barcode width level 2 <= width <= 6 If you do not set the default barcode width level to 3

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setBarCodeWidth(4);


**16ã€Set the height of the barcode**

Functionï¼švoid setBarCodeHeight(int height)

parameterï¼š

        1. height     -->  barcode height 1 <= height <= 255 Same as above, every 8 points is 1mm

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setBarCodeHeight(100);

**17ã€When printing barcodes, select the printing position for HRI characters**

Functionï¼švoid setBarCodeContentPrintPos(int position)

parameterï¼š

        1. position   -->  position HRI character printing position

                                0 --> Do not print

                                1 --> Above the barcode

                                2 --> Below the barcode

                                3 --> Barcodes are printed above and below

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setBarCodeContentPrintPos(2);

**18ã€Print barcode**

Functionï¼švoid printBarCode(int barCodeType, String barCodeContent)
throws UnsupportedEncodingException

parameterï¼š

        1. barCodeType    -->  barcode type 0 <= barcodeType <= 6
        2. barCodeContent -->  Printed barcode character content

    note::

        barcodeType value   Supported barcode content length                    Supported ASCII code range
        0 --> UPC-A            barCodeContent.length = 11,12                            48 â‰¤rangeâ‰¤ 57
        1 --> UPC-E            barCodeContent.length = 11,12                            48 â‰¤rangeâ‰¤ 57
        2 --> JAN13 / EAN13    barCodeContent.length = 12,13                            48 â‰¤rangeâ‰¤ 57
        3 --> JAN8 / EAN8      barCodeContent.length = 7,                               48 â‰¤rangeâ‰¤ 57
        4 --> CODE39           barCodeContent.length >=1,                       48â‰¤rangeâ‰¤57,65â‰¤rangeâ‰¤90,range = 32, 36, 37, 42, 43, 45, 46, 47
        5 --> ITF              barCodeContent.length >=2                                48 â‰¤rangeâ‰¤ 57
        6 --> CODABAR          barCodeContent.length >=2,                       48â‰¤rangeâ‰¤57, 65â‰¤rangeâ‰¤68,97â‰¤rangeâ‰¤100,range = 36, 43, 45, 46, 47, 58

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printBarCode(2 ,"0123456789");

**19ã€Print barcode**

Functionï¼švoid printBarCode(int barCodeType, String barCodeContent, int alignmentMode)
throws UnsupportedEncodingException

parameterï¼š

        1. barCodeType    -->  barcode type 0 <= barcodeType <= 6
        2. barCodeContent -->  Printed barcode character content
        3. alignmentMode  -->  0 = Left / 1 = Center / 2 = Right

    note::

        barcodeType value   Supported barcode content length                    Supported ASCII code range
        0 --> UPC-A            barCodeContent.length = 11,12                            48 â‰¤rangeâ‰¤ 57
        1 --> UPC-E            barCodeContent.length = 11,12                            48 â‰¤rangeâ‰¤ 57
        2 --> JAN13 / EAN13    barCodeContent.length = 12,13                            48 â‰¤rangeâ‰¤ 57
        3 --> JAN8 / EAN8      barCodeContent.length = 7,                               48 â‰¤rangeâ‰¤ 57
        4 --> CODE39           barCodeContent.length >=1,                       48â‰¤rangeâ‰¤57,65â‰¤rangeâ‰¤90,range = 32, 36, 37, 42, 43, 45, 46, 47
        5 --> ITF              barCodeContent.length >=2                                48 â‰¤rangeâ‰¤ 57
        6 --> CODABAR          barCodeContent.length >=2,                       48â‰¤rangeâ‰¤57, 65â‰¤rangeâ‰¤68,97â‰¤rangeâ‰¤100,range = 36, 43, 45, 46, 47, 58

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printBarCode(2 ,"0123456789", 1);

**20ã€Set the size of the QR code**

Functionï¼švoid setQrCodeSize(int level)

parameterï¼š

        1. level     -->  QR code block size, unit: dot, 1 <= level <= 16

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setQrCodeSize(2);

**21ã€Set QR code error correction**

Functionï¼švoid setQrCodeErrorCorrectionLev(int level)

parameterï¼š

        1. level     -->  level >= 48 && level <= 51

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setQrCodeErrorCorrectionLev(51);

**22ã€Set left margin of barcode and QR code**

Functionï¼švoid setLeftMargin(int marginValue)

parameterï¼š

        1. marginValue     -->  Left Spacing Value 0-576

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setLeftMargin(100);


**23ã€Printer QR code**

Functionï¼švoid printQrCode(String  qrStr)

parameterï¼š

        1. qrStr     -->  QR code content.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printQrCode("https://www.imin.sg");

**24ã€Printer QR code**

Functionï¼švoid printQrCode(String  qrStr, int alignmentMode)

parameterï¼š

        1. qrStr                -->  QR code content.
        2. alignmentMode        -->  0 = Left / 1 = Center / 2 = Right.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printQrCode("https://www.imin.sg", 1);

**25ã€Set paper specifications**

Functionï¼švoid setPageFormat(int style)

parameterï¼š

        1. style     -->  0-80mm    1-58mm

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).setPageFormat(1);

**26ã€Print bitmap**

Functionï¼švoid printSingleBitmap(Bitmap bitmap)

parameterï¼š

        1. bitmap     -->  bitmap

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printSingleBitmap(bitmap);

**27ã€Print bitmap**

Functionï¼švoid printSingleBitmap(Bitmap bitmap, int alignmentMode)

parameterï¼š

        1. bitmap     -->  Bitmap
        2. alignmentMode        -->  0 = Left / 1 = Center / 2 = Right.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printSingleBitmap(bitmap, 1);

**28ã€Print multiple bitmaps**

Functionï¼švoid printMultiBitmap(List<Bitmap> bitmaps)

parameterï¼š

        1. bitmap     -->  bitmaps

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printMultiBitmap(bitmaps);

**29ã€Print multiple bitmaps**

Functionï¼švoid printMultiBitmap(List<Bitmap> bitmaps, int alignmentMode)

parameterï¼š

        1. bitmap     -->  bitmaps
        2. alignmentMode        -->  0 = Left / 1 = Center / 2 = Right.

.. code-block:: java
        :linenos:
        :emphasize-lines: 1

        IminPrintUtils.getInstance(TestPrintActivity.this).printMultiBitmap(bitmaps, 1);

2.Use the built-in virtual blueprint to call the printer
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

**Introduction to Virtual Blue**

In the bluetooth device list, you can see a paired and always-existing bluetooth device "BluetoothPrinter", which is a printer device virtualized by the operating system and does not actually exist. Some of these special commands are iMin self-defined commands, such as:


Use of Virtual Blue

1. Establish a connection with the Bluetooth device.

2. Splice the instructions and text into Bytes.

3. Send to BluetoothPrinter.

4. The underlying printing service drives the printing device to complete printing.
