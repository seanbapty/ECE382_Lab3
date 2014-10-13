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
