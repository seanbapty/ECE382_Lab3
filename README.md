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
##Objectives
The goal of this lab was to use the logic analyzer to examine how the MSP430 handles periferals, and modify code that draws a line on a LCD when a button is pressed to draw an 8x8 box on the LCD when a button is pressed.
##Preliminary design
To get started using the logic analyzer and determining which section of code needed to be modified, the Physical Communication and Writing Modes were analyzed.
###Physical Communication
The code provided in the lab called NokiaByte 4 times in order to sent packets of data to the display and draw a line. The table below summarizes these calls.

![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/lab%20table%201.JPG)

Additionally, the table below summarizes the 9 bit packets (reduced to 8 bit by seperating the MSB) analyzed by the logic analyzer. These packets are representative of the first line drawn on the LCD in the upper right corner.

![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/lab%20table%202.JPG)

###Logic Analyzer
Two simulations were run on the logic analyzer. The first simulation was run to measure the time gap between command/data bit and the data bits and measure the characteristics of the microcontroller during a draw line putton press. The specific value of the sources and the time gap is in the chart below.

![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/logic%20analyzer%20command.jpg)

The second simulation was run to demonstrate the behavior during a reset button press. The specific value of the sources and the time gap is in the chart below.
![alt tag](https://github.com/seanbapty/ECE382_Lab3/blob/master/logic%20analyzer%20reset.jpg)

###Writing Modes
As a demonstration of logical operators the chart below was made.

![alt tag](https://raw.githubusercontent.com/seanbapty/ECE382_Lab3/master/andOrXor.JPG)

##Code
The code provided by Dr. Coulston created a line on the LCD screen 8 pixels vertical with a one pixel hole in the centre. This code could be easily modified to draw an 8x8 box given that a box is 8 of these lines stacked horizontally with the hole in the middle removed. The code below loops through the call to #writeNokiaByte 8 times in order to draw the 8 lines. Additionally, the value of 0xFF was set to regiser 13 so that the line was solid.

```
mov		#8, R7						; to draw a box 8x8 r7 counts number of iterations
boxloop
	mov		#NOKIA_DATA, R12			; loops through drawing an 8 pixel vertical line 8 times offset 1 bit horizontally
	mov		#0xFF, R13					;
	call	#writeNokiaByte
	dec 	R7
	jnz		boxloop
```
##Debugging
The code functioned the first time it was tested so there was no need to debug. A potential error that could have happend is the assingment of the wrong value to r13 should the programmer not realize that to draw a solid line the binary sent to the LCD must be all logical hight (pixel on) or 1111 1111 (0xFF).
##Testing/Results
To test the code the MSP430 was plugged into the Nokia LCD, and the code was run. While running, the user pressed the set input button and an 8x8 pixel box was drawn on the screen. In summary, the inputs to the LCD were 1111 1111 when the button was pressed (as determined by line 116 in the code above), and the output was an 8x8 pixel box on the screen. The size of the box was confirmed by counting the pixels of the output. Additionally, after each subsequent press a box was drawn below the first box one pixel to the right. Functionality of the Lab did not state anything about the location of the box so this code completly functional.
##Conclusion
Periferals are useful attachments to microcontrollers because they allow input and output to/from the outside world. This lab demonstrated how user input (the press of a button) can be interpreted by a microcontroller and lead to useful output (a box drawn on the screen). Should this lab be conducted again, it would be useful to incorporate the directional buttons on the LCD to make the box move, or develop a simple game such as "Pong" on the LCD. 
####Documentation
#####Prelab
The following cadets worked together and discussed Lab 3 Mega Prelab on Sunday, 28 September 2014 starting at 1830 in the 321 classroom. 
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
#####Lab
In order to determine the location of the read/write NokiaByte in the first table of the lab, I asked Capt. Trimble and C2C Jasper Arneberg. Additionally, I checked the accuracy of my lab tables against those posted on C2C Arenberg's github. Finally, C2C Gruska explained the basic principal behind changing the value of r13 to make the line solid, and that a loop could be included to draw 8 lines side by side.
