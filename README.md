ECE382_Lab3
===========
#Pre Lab
From the Nokia1202 LCD BoosterPack v4-5, whcih was used in this lab, the following table was completed.
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/prelab%20table%201.JPG)

As an additional prelab activity, students were required to complete the following table identifying registers based on the following code.
```
mov.b    #LCD1202_CS_PIN|LCD1202_BACKLIGHT_PIN|LCD1202_SCLK_PIN|LCD1202_MOSI_PIN, & A
mov.b    #LCD1202_CS_PIN|LCD1202_BACKLIGHT_PIN|LCD1202_SCLK_PIN|LCD1202_MOSI_PIN, & B
mov.b    #LCD1202_RESET_PIN, & C
mov.b    #LCD1202_RESET_PIN, & D
```
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/prelab%20table%202.JPG)

Next, students identified the function of individual bits in the following code. The results are in the table below.
```
bis.b    #UCCKPH|UCMSB|UCMST|UCSYNC, &UCB0CTL0
bis.b    #UCSSEL_2, &UCB0CTL1
bic.b    #UCSWRST, &UCB0CTL1
```
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/prelab%20table%203.JPG)

The thrid part of the prelab was to draw a timing diagram for the LCD1202_CS_PIN, LCD1202_SCLK_PIN, LCD1202_MOSI_PINs. This timing diagram is unavialable at this time as it was turned into Capt. Virginia Trimble. If this diagram is needed, try contacting Capt. Trimple at virginia.trimble@usafa.edu.

The last part of the pre lab was to complete the table based on the Nokia1202 display managed by the following code.
```
;-------------------------------------------------------------------------------
;    Name:        initNokia        68(rows)x92(columns)
;    Inputs:        none
;    Outputs:    none
;    Purpose:    Reset and initialize the Nokia Display
;-------------------------------------------------------------------------------
initNokia:

    push    R12
    push    R13

    bis.b    #LCD1202_CS_PIN, &P1OUT

    ;-------------------------------------------------------------------------------
    ; Measure the time that the RESET_PIN is held low by the delayNokiaResetLow loop
    bic.b    #LCD1202_RESET_PIN, &P2OUT
    mov    #0FFFFh, R12
delayNokiaResetLow:
    dec    R12
    jne    delayNokiaResetLow
    bis.b    #LCD1202_RESET_PIN, &P2OUT
    ;-------------------------------------------------------------------------------

    mov    #0FFFFh, R12
delayNokiaResetHigh:
    dec    R12
    jne    delayNokiaResetHigh
    bic.b    #LCD1202_CS_PIN, &P1OUT

    ; First write seems to come out a bit garbled - not sure cause
    ; but it can't hurt to write a reset command twice
    mov    #NOKIA_CMD, R12
    mov    #STE2007_RESET, R13                    ; DECODE HERE
    call    #writeNokiaByte


    mov    #NOKIA_CMD, R12
    mov    #STE2007_RESET, R13
    call    #writeNokiaByte

    mov    #NOKIA_CMD, R12
    mov    #STE2007_DISPLAYALLPOINTSOFF, R13            ; DECODE HERE
    call    #writeNokiaByte

    mov    #NOKIA_CMD, R12
    mov    #STE2007_POWERCONTROL | STE2007_POWERCTRL_ALL_ON, R13    ; DECODE HERE
    call    #writeNokiaByte

    mov    #NOKIA_CMD, R12
    mov    #STE2007_DISPLAYNORMAL, R13                ; DECODE HERE
    call    #writeNokiaByte

    mov    #NOKIA_CMD, R12
    mov    #STE2007_DISPLAYON, R13                    ; DECODE HERE
    call    #writeNokiaByte

    pop    R13
    pop    R12

    ret
```
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/prelab%20table%204.JPG)

#Lab
##Physical Communication
The code provided in the lab called NokiaByte 4 times in order to sent packets of data to the display and draw a line. The table below summarizes these calls.
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/lab%20table%201.JPG)

Additionally, the table below summarizes the 9 bit packets (reduced to 8 bit by seperating the MSB) analyzed by the logic analyzer. These packets are representative of the first line drawn on the LCD in the upper right corner.
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/lab%20table%202.JPG)
##Writing Modes
As a demonstration of logical operators the chart below was made.
![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/andOrXor.JPG)
####Documentation
DOCUMENTATION: The following cadets worked together and discussed Lab 3 Mega Prelab on Sunday, 28 September 2014 starting at 1830 in the 321 classroom. 
•	C2C Nathan Ruprecht
•	C2C Erik Thompson
•	C2C Austin Bolinger
•	C2C Sabin Park
•	C2C Kyle Jonas
•	C2C Jeremy Gruszka
•	C2C Kevin Cabusora
•	C2C Taylor Bodin
•	C2C Jarrod Wooden 
•	C2C Sean Bapty
•	C2C Erica Lewandowski
•	C2C Chris Kiernan
•	C2C JP Terragnoli
•	C2C Hunter Her
•	C2C Gytenis Borusas

