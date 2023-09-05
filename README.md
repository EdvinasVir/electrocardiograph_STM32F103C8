# electrocardiograph_STM32F103C8

**Device overview**

A DIY electrocardiograph used to monitor heart rate and calculate beats per minute (BPM). The biomedical sensors are connected to body in three places (right arm, left arm and right leg). The signal is sent to AD8232 heart rate monitor amplifier where the signal is filtered and amplified before being sent to the STM32f103c8t6 microcontroller. The heart activity and the BPM is visualised via screen. Project was coded using STM32Cube IDE and C programming language.  A link to a video demonstration: https://youtu.be/xl9m9C466GI

**Parts**
- STM32f103c8t6 microcontroller
- AD8232 heart rate monitor
- 2.2 TFT LCD screen

**Libraries used**

HAL ILI9341 library was used to draw screen contents, a link to the library: https://github.com/eziya/STM32_HAL_ILI9341
ILI_9341_DrawLine() function was borrowed from https://github.com/ardnew/ILI9341-STM32-HAL

**Code overview**

- The microcontroller peripherals are setup to capture the signal using built-in ADC and interface with ILI9341 tft lcd chip driver using SPI and DMA.
- The main function calls initialisation functions, draws a 'bpm value' text to the screen and jums to an infinite while(1) loop. Inside the while loop, screen flag           controlls the screen update frequency. The screen flag is set using timer interrupts to avoid updating screen more frequently than it is necessary. When the flag is set,    the ADC signal is normalised between 0 and 215 to fit inside the screen. The value is then checked using if statements to detect peaks and set the beats per minute (bpm)    flag. The ILI9341_DrawLine function from the library is used to draw screen contents, x position is updated and checked using if's to avoid drawing off-screen. Screen       flag is reset to zero.


**Issues**
1. BPM value fluctuation due to high level of noise (noise spikes are registered as a heartbeat). This problem arises due to high amount of electromagnetic activity around the system. There are many ways to fix this problem.
   - One is to implement an additional low-pass filter stage after the signal is amplified and filtered using AD8232.
   - The other way to fix this problem is to add a filtering stage using software. After the microcontroller receives a signal, the low-pass filter can reduce
     high frequency content.
   - Use shielded cable to reduce the amount of electromagnetic noise captured by cables.
