<html>
<body>

  <h1>PSXTAL DFO - Dual Frequency Oscillator for PS1</h1>

<br>
  
Goes well with my [In Game Reset!](https://github.com/L10N37/PS1-IGR-in-game-reset-PlayStation-Sony-psone)

<br>

![SI5351](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/SI5351Header.png?raw=true)

<br>
<br>

  <h1>Board Measurements:</h1>
  <p>Approximately 18.5mm * 18.5mm</p>

  ![PCB](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/PCB_Image.png?raw=true)
  ![PCB](https://github.com/L10N37/DFO-PS1-PSXTAL/blob/main/V1_installed_top_mount.jpg?raw=true)
<br>
<br>

 <h1>PSXTAL DFO V2 Update / Notes</h1>

 ![PCB](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/V2_Update/PCB_Image.png?raw=true)

Options for Reference clock for SI5351-

The 25MHz 3225 reference oscillators come with a few different layouts, the original version 1 board suited JP2, 

JP1: This is for E/D, OE or ST as oscillator pin 1 (Enable Control/ Enable Disable / Output Enable), this ties pin 1 to VCC but often you can leave this pad tri-state / floating, which the board will do by default. 

JP2: This is for oscillators that have pin 1 as a ground, this ties pin 1 to ground. These often work floating (default)

In most cases, you can leave the board as is with pin 1 floating,

I grabbed a reference XTAL from a different bag of 25MHz 3225 XTALS on my last assembly of the board and this shorted out GND to the 3.3v rail. Annoying as hell, the 0603 footprints were way too small and I was installing 0402 components on them instead, this was still difficult. I assumed when I made the version 1 of the board they'd be the same size 0603 footprints I'd used on other boards. They definitely were not. They are now whopping 0805 footprints which should fit 0805, 0603 and 0402 components.


Bypass clock out resistor

JP3 -clock out resistor bypass: You don't need the output resistor for a top mount installatoin if you leave the factory one in place and solder to the old oscillator pads (preferred installation method). For bottom mount installations you will want to leave this open and install the resistor on the DFO board.


A test pad has being added to check the reference clock out with a scope and make sure it's good as the SI5351 pins are tiny and difficult to probe.


  <h1>Components:</h1>
  <ul>
    <li><span class="component">2 * 1k to 10k 0603 resistors:</span> For I2C communication line pullups (SDA/SCL).</li>
    <li><span class="component">1 * Atmega 328P MCU:</span> Clones from Ali are good in my experience. For flashing with the psxtal software that runs the show.</li>
    <li><span class="component">1 * SI5351 clock board, MSOP10 footprint:</span> Ali ones are fine in my experience, though I've read about bad batches. For generating the PS1 GPU clock frequency with glitchless frequency switching.</li>
    <li><span class="component">4 * 100nf (0.1uf) 0603 ceramic capacitors:</span> For bypass capacitors -- 2 for the MCU, 1 for the clock generator IC, 1 for the PLL/Reference oscillator.</li>
    <li><span class="component">1 * 220r 0603 resistor (bypass for top mount):</span> In series on the clock out as per PS1 GPU clock. Originally jumpered (prototype) in case you wanted to top mount / replace the factory GPU oscillator. Instead, we can render that clock useless by removing the on-board 220r resistor on its clock output. I would still personally be removing the factory oscillator completely with hot air.</li>
    <li><span class="component">1 * 25Mhz SMD oscillator:</span> This is used by the SI5351 clock gen IC as a reference clock, please check jumper options</li>
  </ul>

<br>
<br>

  <h1>Flashing</h1>

The board doesn't have any pin headers for any type of ISP flashing. Regular flashing over UART is out of the question as the bare IC's don't come with a bootloader pre-flashed. Instead, you use this (or similar):

![FLASHER](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/Flashing/TQFP32-to-DIP28.png?raw=true)

This is used in conjunction with an XGecu Pro TL866II Plus to flash the MCU with [this Hex File](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/HEX/PSXTAL-DFO.hex) from the original [PSXTAL Repo](https://github.com/L10N37/PSXTAL/tree/main). The original project uses off the shelf parts, installation can be a little messy compared to this custom PCB solution.

<br>
<br>

<h1> Fuse Settings </h1>

  ![FUSES](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/Flashing/FuseSettings.png?raw=true)

<br>
<br>

 <h1> Accuracy </h1>

The project uses the Adafruit SI5351 library, and the math for the clocks was calculated with pen, paper, and a calculator, along with a little trial and error. The original project was installed in many pre-modded systems and tested across many CRTs in my collection on S-video and composite out. Though I've encountered ZERO issues, and neither have my customers - if you can land closer to the exact frequencies, let me know.<br>


 PAL:
 For PU8 / PU18, this frequency divided by 12 equates to your colourburst frequency.<br>
 Actual: 4.43361875 MHz <br>
 This Board: 4.4335 Mhz

 ![PAL](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/scope/SDS00001.png?raw=true)

<br>
<br>

 NTSC: 
 For PU8 / PU18, this frequency divided by 15 equates to your colourburst frequency.<br>
 Actual: 3.57954 MHz <br>
 This Board: 3.57936Mhz

 ![NTSC](https://github.com/L10N37/PSXTAL/blob/main/PSXTAL-DFO/scope/SDS00002.png?raw=true)

<br>
<br>

  <h1>Installation</h1>
  <p>It's no different from other DFO's, it uses the same points:</p>
  <ul>
    <li>~3.5v for power</li>
    <li>Ground</li>
    <li>Sense (SO)</li>
    <li>Clock Out</li>
  </ul>
<br>
I would suggest using another install diagram, but buzzing out the points to ones that are more suitable for this boards solder pads. Often you will end up scraping back the green layer on the PCB to create a point.

<br>
<br>

   <h1> Technical PS1 GPU clock Stuff </h1>

  The PU8 and PU18 versions of the Sony Playstation use a GPU oscillator to produce the GPU clocks required for colour generation and screen refresh rate. The CPU clock is generated by a separate oscillator.

  * 53.2034 MHz is for PAL (Europe, Australia, New Zealand and parts of Asia.)
  * 53.693175 MHz is for NTSC (North America, Japan)

  PAL Consoles always start with sense pin high (NTSC mode momentarily) before the BIOS kicks in and initialises the screen. Shortly after power-up, this signal/ pin is pulled to ground and the console will divide the GPU clock by 12. This gives you the 4.43Mhz you need to produce PAL colours on your set when using S-Video or Composite output. The problem when running an NTSC game on these consoles, is that the console will divide the GPU clock by 15 in NTSC mode. Using an approximation of  53.2 / 15, this gives you about a 3.54Mhz subcarrier signal. This isn't a valid clock signal and you get a black and white picture.
<br>
<br>
<h2> Old School Fix  </h2>
<br>  

  The 'old-school' fix to get colour was to take the video encoders sub carrier input pin, lift it off the board and feed it a perma-4.43Mhz PAL colour sub-carrier clock signal. This way, regardless of the PS1 being in PAL or NTSC Mode, you would still have the PAL sub carrier required to give you a colour picture. the *problem* with this is that, these days - scalers are very popular devices. Now when the console is in NTSC mode and using this 'old-school' sub carrier modification, we get around a 59.24hz refresh rate. The actual 60hz refresh rate should be 59.94Hz. This discrepancy causes frame drops, stuttering and other issues when combining this fix with a scaler. For CRT's, the small discepancy is a non issue. When using RGB, you'll get colour regardless (no fix required), but the small clock discrepancies are still prevelant.

<br>
<br>
<h2> Modern Fix  </h2>
<br>

Nowadays, we can use nifty modern ICs to simply toggle between the 2 GPU clocks. This gives you 'true' NTSC and PAL modes with their respective 'true' subcarrier clocks. The console is now truly multi-region. There are only 2 or 3 NTSC games that won't work on a PAL console (some have patches) due to the crafty developers adding code to detect the mechacon in the console! Unfortunately, PAL consoles still have PAL mechacons and NTSC consoles have NTSC mechacons. They only seem to check for NTSC/PAL and nothing in the BIOS, as the US games mentioned work on JP consoles.

<br>
<br>
<h2> What about the later models? PU20, PU22, PU23 and PM41 revisions? </h2>
<br>

Sony switched from using oscillators to produce the clocks, to instead, using a clock generator IC with PLL reference clock. See here.

![NewModels](https://github.com/L10N37/PSXTAL/blob/main/PU22+/PU23ClockGen.png?raw=true)

A PCB will be designed in the future to cover the Sub-Carrier clock outputs, along with the GPU clock outputs. I do have [PSXTAL 3FO](https://github.com/L10N37/PSXTAL/blob/main/PU22%2B/PU23ClockGen.png), which outputs a very slightly overclocked CPU, along with GPU clocks and Sub-Carrier clocks that toggle on sense. I didn't have a 100% success rate with PAL mode on this project. It was probably because of using a different PLL to produce the GPU than the Sub carrier in PAL mode. It does work, but some sets will be black and white in PAL mode. It needs a re-visit.

   <h1>Notes and support</h1>

Since losing some of my KiCad files and not being able to make modifications to old projects, I choose to share them openly. It benefits me, and at the same time, if anyone is interested, they can jump in and do their own thing. I don't care if you make these and sell them for profit. Do whatever you want. I am happy to assist if you have any questions. This guide is not for beginners. Note that the resistors and capacitors have no wiggle room and are not easy to hand solder. A hot air station will come in handy.

  [License: Creative Commons Attribution-ShareAlike (CC BY-SA)](https://creativecommons.org/licenses/by-sa/4.0/)

</body>
</html>
