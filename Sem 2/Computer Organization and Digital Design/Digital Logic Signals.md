Digital Logic Signals
#### **Analog vs Digital Devices**

- Analog devices and systems have time varying signals that can take continuous ranges of values
- Digital devices and systems, we pretend there are only two states (0 or 1 / High or Low)

- In terms of speed for calculations, **digital devices** generally have an advantage over analog devices.

|                    | Analog                                                                                                            | Digital                                                                                                                                      |
| ------------------ | ----------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- |
| Example            | Human voice in air, analog electronic devices.                                                                    | Computers, CDs, DVDs, and other digital electronic devices.                                                                                  |
| Data transmissions | Subjected to deterioration by noise during transmission and write/read cycle.                                     | Can be noise-immune without deterioration during transmission and write/read cycle.                                                          |
| Response to Noise  | More likely to get affected reducing accuracy                                                                     | Less affected since noise response are analog in nature                                                                                      |
| Uses               | Can be used in analog devices only. Best suited for audio and video transmission.                                 | Best suited for Computing and digital electronics.                                                                                           |
| Bandwidth          | Analog signal processing can be done in real time and consumes less bandwidth.                                    | There is no guarantee that digital signal processing can be done in real time and consumes more bandwidth to carry out the same information. |
| Memory             | Stored in the form of wave signal                                                                                 | Stored in the form of binary bit                                                                                                             |
| Power              | Analog instrument draws large power                                                                               | Digital instrument draws only negligible power                                                                                               |
| Cost               | Low cost and portable                                                                                             | Cost is high and not easily portable                                                                                                         |
| Impedance          | Low                                                                                                               | High order of 100 megaohm                                                                                                                    |
| Errors             | Analog instruments usually have a scale which is cramped at lower end and give considerable observational errors. | Digital instruments are free from observational errors like parallax and approximation errors.                                               |

#### Logic Levels

The voltages used to represent a 1 and 0 are called logic levels
E.g.   CMOS (LV) 
	(2V to 3.3V HIGH)
	(0V. To 0.8V LOW)


![[PdfImage.png]]

Duty Cycle ( Assuming a periodic waveform) :

Ratio of the pulse width (tw) to the period (T)

$$
Duty cycle = ( tw / T ) * 100%
$$ 
##### Logic Families 
 
Chips from the same family can be interconnected to perform any desired logic function

Chips from different logic families may not be compatible, so we need to take special steps to interconnect circuits from different logic families

##### Different Levels of Abstraction

- Simulate circuits at digital logic level 
- Implement using set of readily available hardware building blocks
- Simulate circuits at digital logic level 

Can we treat hardware design as programs? 
	Yes, hardware design can indeed be treated as programs. This approach is known as **Hardware Description Language (HDL)** programming.
	
A Field-Programmable Gate Array (FPGA) is a type of integrated circuit that can be programmed or reprogrammed after manufacturing.


##### Data Transfer 

Binary data are transferred in two ways:

| **Serial Transmission**                                       | **Parallel Transmission**                              |
| ------------------------------------------------------------- | ------------------------------------------------------ |
| bits are sent one at a time in a sequence over a single line. | all the bits in a group are sent out on separate lines |
| over long distances                                           | short distances                                        |
| USB, Ethernet, and Bluetooth                                  | data bus in a computer.                                |

[[Number systems]]