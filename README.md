
# Hey Dad, Listen To Me !

### Table of Contents  <br /> <br />



## Motivation, It's all about the family connection :family:

Due to COVID-19 situation, a lot of families shift to working from home and long distance learning. Working parents and kids share the same space for working and studying. 
For parents, it's good to take care that your kids belong with you but sometimes that's very challenging for me to switch work and family mode in the house. <br /><br />
Thanks to the high internet bandwidth at home, you can actually do all of your work at home with people in different time zone. <br /> 
That means you could sit on your dining table and watch your computer from 7am morning until 9pm for one and one virtual meeting without looking and talking with your family. :sob: <br /> <br />
Because of that, I'd like to create an interesting interactivity program with kids so that when they need to talk with parents then they can swipe or speak a keyword to some tiny machine to give parents heads up.<br /><br />

Check this video about the details. <br />

<a href="https://www.youtube.com/embed/95lYoM273S4
" target="_blank"><img src="/photos/HeyDad_ListenToMe.PNG" 
alt="Youtube Video"/></a>
<br />
<br />

## Project source
This project majorly based on three of open-source projects: <br />
1. TensorFlow Lite for Microcontrollers (https://www.tensorflow.org/lite/microcontrollers)
2. Tiny Motion Trainer (https://experiments.withgoogle.com/tiny-motion-trainer) and 
3. Raspberry Pi Pico SDK (https://github.com/raspberrypi/pico-sdk).
<br /><br /><br />

## Hardware Board:
1. arduino nano BLE 33 *2:<br />
   * https://store.arduino.cc/usa/nano-33-ble <br />
   * 32-bit ARM® Cortex™-M4 CPU running at 64 MHz, 1MB of program memory. 
   * This CPU should be good enough for TensorFlow Lite for Microcontrollers to detect gestures and speech keyword.

2. micro:bit v2 *2<br />
   * In the beginning, I plan to use arduino bluetooth low energy (LBE) as the communication protocol. But we need extra work to figure out computer or smartphone APP to control the communication, that's difficult to explain with a 9-year-old kid.<br />
   * I'm thinking to use most easy hardware as the primary communication board, so I decided to use micro:bit v2 (https://microbit.org/new-microbit/). Kids can easily add lighting and sound anything they want on scratch. (https://makecode.microbit.org/)
   * Finally, I use a radio signal to communicate between commander and receiver. on micro:bit also provides a lot of funny multimedia extension board like funny voice speaker, led light strips, and servo motor for kids to bring their creativity.<br />
		
3. Raspberry Pi Pico Ardicam *1 <br />
   * Dual-core Arm Cortex-M0+ processor, 133 MHz, 264KB on-chip SRAM, 2MB on-board Flash. https://www.raspberrypi.org/products/raspberry-pi-pico/specifications/
   * CPU without DSP support and SRAM smaller then arduino nano BLE 33. 
   * I got one of Arducam Pico4ML with Raspberry Pi RP2040 processor which embedded QVGA camera so I don't need additional camera. https://www.arducam.com/product/rp2040-based-arducam-pico4ml-dev-board-for-machine-vision/
   * Re-use tflite-micro person detection example to build up my project. (https://github.com/tensorflow/tflite-micro/tree/main/tensorflow/lite/micro/examples/person_detection)

4. micro:bit hardware extension from Seees x Grove (option, you can use any other hardware instead.) <br />
   * https://www.seeedstudio.com/Grove-Inventor-Kit-for-micro-bit-p-2891.html
<br /><br />

## How to Play:
They are two devices: <br />
A. Commander device <br />
B. Receiver device <br />
<img src="/photos/commander_device.PNG" width="400px" /> <img src="/photos/receiver_device.PNG" width="400px" />

<br /><br />

1. When a kid wants parents to listen to them, they will swipe the commander device or speech a key-word to the commander device. <br />
2. Once the device recognizes the gesture or keyword, the commander device will send out a command to the Receiver device. <br />
3. Then, the parent received a sound and also LED lighting from the dining table. The speaker will buzz every few seconds. <br />
4. To stop the buzzing, parents need to put aside the work on hand and move the sightline into the receiver device. <br />
5. The receiver device will calculate the image three times then stop the buzzing. At that time, parents could focus on what the kid what's them talk about.
<br /><br /><br />

## Have fun and hope you can enjoy :blush:
<br />
Check this video --> https://www.youtube.com/embed/95lYoM273S4  <br />
<br />
<br />

## Machine Learning Training
### Gesture define:
When I bring this idea to discuss with the kid, we decide two gestures, time-out and roll.
<br />
#### Time out 
<a href="https://www.youtube.com/embed/aCJEFmkYoiE
" target="_blank"><img src="/photos/time_out_gesture.PNG" 
alt="Time-out Gesture Example" width="480" height="480" border="10" /></a>
<br />

#### Roll
<a href="https://www.youtube.com/embed/iG2-kHf2zCA
" target="_blank"><img src="/photos/roll_gesture.PNG" 
alt="Roll Gesture Example" width="480" height="480" border="10" /></a>
<br />

### Gesture Training:
Kids and I follow up this Tiny Motion Trainer (https://experiments.withgoogle.com/tiny-motion-trainer) to create two gestures. <br />
Collect 50 samples for "Time-Out" <br />
<img src="/photos/motion_training4.PNG" width="1000px" />   
<br />

Then, 50 samples for "Roll"<br />
<img src="/photos/motion_training3.PNG" width="1000px" />   
<br />

Start the training on Google Cloud. <br />
<img src="/photos/motion_training5.PNG" width="1000px" />   
<br />

Verified the trained model, it looks pretty good on my try. :blush: <br />
<img src="/photos/motion_training1.PNG" width="800px" />   
<img src="/photos/motion_training2.PNG" width="800px" />   
<br />

Finally, download the example code into my local machine to add a communication function.
<br /><br /><br />

### Person Detection Training:
Original tflite-micro is work well on Arduino Cortex-M4, so I apply the same int8 model into Pico. The result work well. <br />
https://github.com/tensorflow/tflite-micro/tree/main/tensorflow/lite/micro/examples/person_detection
<img src="/photos/Person_Detection.JPG" width="800px" /> 

Now, we can start the device deploy. <br /><br />

## Device Implementation

Basic on our project spec, there are three of parts need implement. <br /><br />
* arduino gesture detection.
* Pi PICO person detection.
* micro:bit communication and interactive hardware control.

<br />
Let's start from arduino. <br />

### arduino gesture detection
tiny-motion-trainer has already provided arduino code for download, you can based on the example to design for your project.
https://experiments.withgoogle.com/tiny-motion-trainer

[Source Here](./TinyMotion-timeput-roll--v2/DaddyHeadsUp/DaddyHeadsUp.ino)
```c
...
  while (isCapturing) {
    ...
    IMU.readAcceleration(aX, aY, aZ);
    IMU.readGyroscope(gX, gY, gZ);

    // Normalize the IMU data between -1 to 1 and store in the model's
    // input tensor. Accelerometer data ranges between -4 and 4,
    // gyroscope data ranges between -2000 and 2000
    tflInputTensor->data.f[numSamplesRead * 6 + 0] = aX / 4.0;
    tflInputTensor->data.f[numSamplesRead * 6 + 1] = aY / 4.0;
    tflInputTensor->data.f[numSamplesRead * 6 + 2] = aZ / 4.0;
    tflInputTensor->data.f[numSamplesRead * 6 + 3] = gX / 2000.0;
    tflInputTensor->data.f[numSamplesRead * 6 + 4] = gY / 2000.0;
    tflInputTensor->data.f[numSamplesRead * 6 + 5] = gZ / 2000.0;

    if (numSamplesRead == NUM_SAMPLES) {

      // Run inference
      TfLiteStatus invokeStatus = tflInterpreter->Invoke();       
      ...
      for (int i = 0; i < NUM_GESTURES; i++) {
        float _value = tflOutputTensor->data.f[i];
        if(_value > maxValue){
          maxValue = _value;
          maxIndex = i;
        }
      }
      Serial.print("Winner: ");
      Serial.print(GESTURES[maxIndex]);
      
      // GPIO pull-up to trigger micro:bit when maxValue over threshold 

    }
  }
...
```

### Pi PICO person detection.
I follow original arducam guideline to build up my project, mainly on main_functions.cpp.<br />
https://www.arducam.com/raspberry-pi-pico-tensorflow-lite-micro-person-detection-arducam/#person-detection-diagram

[Source Here](./rpi_pico/parent_headsup/main_functions.cpp)
```c
#include "main_functions.h"
#include <LCD_st7735.h>
#include <hardware/gpio.h>
#include <hardware/irq.h>
#include <hardware/uart.h>
#include <pico/stdio_usb.h>

#include "detection_responder.h"
#include "image_provider.h"
#include "model_settings.h"
#include "person_detect_model_data.h"
#include "tensorflow/lite/micro/micro_error_reporter.h"
#include "tensorflow/lite/micro/micro_interpreter.h"
#include "tensorflow/lite/micro/micro_mutable_op_resolver.h"
#include "tensorflow/lite/schema/schema_generated.h"
#include "tensorflow/lite/version.h"

...
void loop() {
  ...
  if (kTfLiteOk != interpreter->Invoke()) {   
    TF_LITE_REPORT_ERROR(error_reporter, "Invoke failed.");
  }
  TfLiteTensor *output = interpreter->output(0);
  ...
  // Process the inference results.
  int8_t person_score    = output->data.uint8[kPersonIndex];
  int8_t no_person_score = output->data.uint8[kNotAPersonIndex];
  RespondToDetection(error_reporter, person_score, no_person_score);
  
  // GPIO pull-up to trigger micro:bit
}

```

### Communication Code Behavior 
Kid and I work this part as well. <br />
We use several hardware extension module to create light and sound to get partner's attention.<br /><br />

#### ListenMe_Commander

microbit-ListenMe_Commander_1 <br />
[Source Here](./microbit/microbit-ListenMe_Commander_1.hex)
```python
def on_button_pressed_ab():
    radio.send_number(trigger)
input.on_button_pressed(Button.AB, on_button_pressed_ab)

trigger = 0
radio.set_group(88)
trigger = 8
basic.show_string("COMMANDER ")

def on_forever():
    if pins.digital_read_pin(DigitalPin.P2) == 1:
        radio.send_number(trigger)
        basic.show_icon(IconNames.SMALL_HEART)
        soundExpression.giggle.play_until_done()
    elif pins.digital_read_pin(DigitalPin.P1) == 1:
        soundExpression.happy.play_until_done()
        basic.show_icon(IconNames.HEART)
    else:
        basic.show_leds("""
            . . . . .
                        . . . . .
                        . . # . .
                        . . . . .
                        . . . . .
        """)
basic.forever(on_forever)

```
#### ListenMe_Receiver
microbit-ListenMe_Receiver_1 <br />
[Source Here](./microbit/microbit-ListenMe_Receiver_1.hex)
```python
def on_received_number(receivedNumber):
    global ack_cont, display, command
    if receivedNumber != command and receivedNumber == trigger:
        strip.show_rainbow(1, 300)
        ack_cont = 0
        display = 60
        command = trigger
        basic.pause(100)
    basic.show_number(command)
radio.on_received_number(on_received_number)

def on_button_pressed_a():
    global status
    if status == 0:
        soundExpression.happy.play()
        status = 1
    else:
        status = 0
        strip.show_rainbow(1, 360)
input.on_button_pressed(Button.A, on_button_pressed_a)

def on_button_pressed_ab():
    global command
    command = acked
input.on_button_pressed(Button.AB, on_button_pressed_ab)

def on_button_pressed_b():
    global ack_cont, display, command
    strip.show_rainbow(1, 300)
    ack_cont = 0
    display = 60
    command = trigger
    basic.pause(100)
input.on_button_pressed(Button.B, on_button_pressed_b)

status = 0
display = 0
ack_cont = 0
acked = 0
trigger = 0
command = 0
strip: neopixel.Strip = None
strip = neopixel.create(DigitalPin.P0, 30, NeoPixelMode.RGB)
_4digit = grove.create_display(DigitalPin.P1, DigitalPin.P15)
strip.show_rainbow(1, 360)
radio.set_group(88)
command = 0
trigger = 8
acked = 9

def on_forever():
    global display, ack_cont, command
    if command == trigger:
        basic.show_icon(IconNames.SAD)
        _4digit.show(display)
        music.play_tone(262, music.beat(BeatFraction.QUARTER))
        music.play_tone(262, music.beat(BeatFraction.QUARTER))
        display = display - 1
        strip.show_color(neopixel.colors(NeoPixelColors.RED))
        if pins.digital_read_pin(DigitalPin.P2) == 1:
            ack_cont = ack_cont + 1
            music.play_tone(523, music.beat(BeatFraction.QUARTER))
        if ack_cont >= 3:
            command = acked
        else:
            if ack_cont / 2 == 0:
                basic.show_icon(IconNames.TARGET)
            else:
                basic.show_icon(IconNames.SMALL_DIAMOND)
    elif command == acked:
        soundExpression.happy.play()
        strip.show_rainbow(1, 360)
        basic.show_icon(IconNames.HAPPY)
        command = 0
        _4digit.show(0)
    else:
        strip.rotate(1)
    basic.pause(1000)
basic.forever(on_forever)
```

All set.
After download the code and plug the USB cable, you can have fun with your kid at home now. :heart_eyes:



