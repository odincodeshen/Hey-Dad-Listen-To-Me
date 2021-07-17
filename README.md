# Hey Dad, Learn To Me !

Due to COVID-19 situation, lot of family shift to working from home and long distance learning, parents and kids share the same space for working and studing.
For parents, it's good to take care you kids belong with you but some of time that's very challenge for me to switch work mode and family mode in the house.
Thanks for the high internet bandwidth at home, you can actually did all of your work at home with people around the world, that's mean you could be possible to sit on your dining table yo watch you computer from 7am morning until 9pm for one and one conf. calls without look and talk with your family.
Because that, I'd like to create an interest interactivity program with kid then when they need to talk with parents then they can swipe or speech a key word to some of tiny machine to give parents heads up.

# Project source
The project based on some of open-source project from Tiny Motion Trainer (https://experiments.withgoogle.com/tiny-motion-trainer), TensorFlow Lite for Microcontrollers (https://www.tensorflow.org/lite/microcontrollers) and Raspberry Pi Pico SDK (https://github.com/raspberrypi/pico-sdk).

# Hardware Board:
1. arduino nano 33 *2
   Use TFLu to detection gesture and key-word.
2. micro:bit v2 *2
   Communicate between commander and receiver. micro:bit provide a lot of funny multimedia widget like funny voice tone, led light strips and digital LED, so increase kid's interesting.
3. Raspberry Pi Pico Ardicam *1 (https://www.arducam.com/raspberry-pi-pico-tensorflow-lite-micro-person-detection-arducam/)
   Use TFLu to person detection. (https://github.com/odincodeshen/tflite-micro/tree/main/tensorflow/lite/micro/examples/person_detection)
   Arduino nano doesn't support camera, so I use one of Pi PICO module from arducam. 
4. micro:bit hardware extension from Seees x Grove
   https://www.seeedstudio.com/Grove-Inventor-Kit-for-micro-bit-p-2891.html

# How to Play:
They are two devices:
A. Commander device
B. Receiver device

1. When kid want parents listen to him/her, kid will swipe commander device or speech a key word to the commander device.
2. Once device recognize the gesture or keywork, commander will send out command to Receiver device.
3. Then, parent received a sound and also LED lighting from the dining table. The speaker will buzzing evry few seconds.
4. To stop the buzzing, parent need put aside the work on hand and move the sight line into the receiver device, the receiver device will calcaute the image three times then stop the buzzing. At that time, parent could focus on what the kid what's the talk about.

# Have fun and hope you can enjoy :)
