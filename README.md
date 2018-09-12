# Practical_4
#!/usr/bin/python

import RPi.GPIO as GPIO
import Adafruit_MCP3008
import time
import os
GPIO.setmode(GPIO.BCM)

#Initilisation
#----------------------------------------------------------------------------
global t1;
t1=0.5
global flag;
flag =0

global timer;
timer = 0
counter = 0

SPICLK = 11 #CLK
SPIMISO = 9 #DOUT
SPIMOSI = 10 #DIN
SPICS = 8 #CEO CS/SHDN
but1 = 26
but2 = 19
but3 = 13
but4 = 21
holding =['']*5
values = [0]*5
disp= holding*5
GPIO.setup(SPIMOSI, GPIO.OUT)
GPIO.setup(SPIMISO, GPIO.IN)
GPIO.setup(SPICLK, GPIO.OUT)
GPIO.setup(SPICS, GPIO.OUT)
GPIO.setup(but1, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(but2, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(but3, GPIO.IN, pull_up_down=GPIO.PUD_UP)
GPIO.setup(but4, GPIO.IN, pull_up_down=GPIO.PUD_UP)

mcp = Adafruit_MCP3008.MCP3008(clk=SPICLK, cs=SPICS, mosi=SPIMOSI, miso=SPIMISO)
#Callbacks
#-------------------------------------------------------------------------
def callback1(channel):
        os.system('clr' if os.name=='nt' else 'clear')
        print('|   Time   |  Timer   | Pot   | Temp  | Light |'.format(*range(5)))
        global timer;
        global flag
        timer = 0

def callback2(channel):
        print("frequency change")
        global t1;
        if (t1==0.5):
                t1=1
        elif (t1==1):
                t1=2
        elif (t1==2):
                t1=0.5

def callback3(channel):
        global flag;
        if (flag==0):
                flag =1
        elif (flag ==1):
                flag=0

def callback4(channel):
        global flag;
        global timer;
        global counter;
        global disp
        counter =0
        print('|   Time   |  Timer   | Pot   | Temp  | Light |'.format(*range(5)))
        for i in range (5):
                 print('| {0:>4} | {1:>4} | {2:>4}V | {3:>4}C | {4:>4}% |'.format(*disp[i]))
                 
GPIO.add_event_detect(but1, GPIO.BOTH, callback=callback1, bouncetime=200)
GPIO.add_event_detect(but2, GPIO.BOTH, callback=callback2, bouncetime=200)
GPIO.add_event_detect(but3, GPIO.BOTH, callback=callback3, bouncetime=200)
GPIO.add_event_detect(but4, GPIO.BOTH, callback=callback4, bouncetime=200)                 
