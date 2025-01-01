# README: Coins ID - Coin Identifier for Visually Impaired People

## Project Description

**Coins ID** is a prototype developed as a technical graduation project in electrotechnics in 2018, aiming to create an accessible device for visually impaired people, allowing them to identify coins through auditory feedback. This document explains in detail how the system works, the programming logic used (in C language), and the main components involved. It's important to note that the project is still in the prototype stage and is not a finished product. Some improvements, such as layout, Braille information, device size, among others, are necessary for the final version.

The main focus of the evaluation was the electrical part of the project, which was successfully completed. Unfortunately, I do not have the code for the project, but the logic will be explained throughout this document.

For better understanding, let's divide the operation into three parts.
---


### 1. Coin Information Collection

**Coins ID** begins by collecting two essential pieces of information from the coin: **size** and **color**. These details are gathered using two components.

#### Potentiometer
A potentiometer is an electronic component that adjusts the electric current based on its resistance. You can adjust its resistance by rotating its axis.  
We attached a rod to the potentiometer's axis. When the coin passes through the rail, it moves the rod, altering the resistance and electrical signal. The larger the coin's diameter, the higher the rod's elevation, and thus, the electrical current is proportional to the coin's size.

![Potentiometer Rod](image/potenciometro1)  
The rod is visible inside the circle.  
![Potentiometer](image/potenciometro2)  
Potentiometer

#### Phototransistor

![Phototransistor](image/fototransistor)

Inside the highlighted circle is the phototransistor, a component that, like the potentiometer, alters the electrical signal. However, it works with exposure to light, changing the signal based on ambient light.  
Do you know the streetlights? Well, there's something similar to this; this is how the streetlight "knows" when it’s day or night, haha.

In our project, it was used to distinguish the color of the coins, as we found that some Brazilian coins are the same size but have different values.

The LED next to it serves to illuminate the cavity where it is located. Through testing, we discovered that when the coin is bronze-colored, the sensor reacts in one way, and when it's silver, it reacts differently. This is because the silver coin reflects more light than the bronze one.

With these two sensors, we obtain two pieces of information: the **size** of the coin and its **color**.

---

### 2. Coin Value Identification

Now that we have the **size** and **color** signals, the **PIC** microcontroller comes into action. It is responsible for processing these signals and identifying the coin.

![Microcontroller](image/microcontrolador)

The **PIC** [Microcontroller](https://en.wikipedia.org/wiki/Microcontroller) is programmed to receive signals from the sensors and compare them to predefined variables in the code. Each coin has a specific size and color value, which was previously mapped. We did not use a database, but rather simple variables to store these values with a margin of tolerance.  
Each sensor is connected to a terminal of the microcontroller. In the code, we receive these values as numerical variables, with integer numbers. These numbers are proportional to the electrical current passing through the terminals—if it's strong or weak, the value increases or decreases.  
Transforming an electrical signal into an integer variable was one of the most challenging parts of the project, especially due to the lack of experience.

P.S.: You may have heard of microcontrollers, but the most famous brand is Arduino.

**Identification Process**:
- The microcontroller receives the signals and converts them into integers, which are proportional to the intensity of the electric current.
- After reading the signals, the values are compared to the predefined values for each coin type (such as 50 cents, 1 real, etc.).
- In the code, we use the famous **IF/ELSE** structure to identify which coin was inserted.

After identification, the microcontroller sends a **binary** signal to the sound system, triggering the corresponding track and also a motor that returns the potentiometer’s rod to its initial position.

---

### 3. Sound Playback

The final stage of the process is the reproduction of the audio that informs the coin's value. For this, we used an **audio integrated circuit**, such as the **ISD-1720**, which is capable of recording and playing back audio tracks.  
I don't exactly remember which component we used, but the ISD-1720 serves the purpose, so I'll use it as an example.

**Sound System Operation**:
- The circuit has control terminals that can be activated by a binary code. Each binary combination triggers a specific audio track.
- Energized terminals represent 1, and non-energized terminals represent 0.
- For example, the fourth track corresponds to the playback of "50 cents," so to select it, we energize only the third control terminal, making it **001**. If I'm not mistaken, there are 4 pins dedicated to controlling the tracks.

![ISD-1720 Sound Circuit](image/som)

---

## Simulation of Operation

Let's illustrate the system's operation with an example of inserting a 50-cent coin:

1. The user passes a 50-cent coin through the device's rails.
2. The **potentiometer** and **phototransistor** capture the coin's size and color, respectively, and send the data to the microcontroller.
3. The microcontroller processes the signals, storing the values in two variables (size and color).
4. The system compares the captured values with the predefined variables and identifies that the data corresponds to the 50-cent coin.
5. The microcontroller sends the binary combination **001** to the audio integrated circuit, which activates the fourth audio track, pronouncing "50 cents."
6. The potentiometer's motor is triggered to return the rod to its initial position.
7. The system is ready to receive the next coin.

---

## License

This project is licensed under the [MIT License](https://opensource.org/licenses/MIT), allowing you to use, modify, and distribute the code freely, as long as the original author is credited.
