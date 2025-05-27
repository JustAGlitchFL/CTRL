---
title: "CTRL"
author: "rowyn"
description: "A split semi-compact keyboard with extra features"
created_at: "2024-05-22"
---

I understand this journal does not fit the format listed on the site, as it is extremely hard for me to record day-by-day progress on the planning and rough designs that I have worked on intermittently since long before Highway started, about a month and a half between V5RC World Championships, AP testing, and graduating from high school. However, the final design was started following the unveiling of Highway.

# The Idea and prior planning
I initially started this project for the HackBoard section of HackPad, but the deadline got pushed and eventually morphed into Highway. 

The aim of this is to create a split keyboard to use with my setup when I move into my college dorm at UF, to better fit my needs over a traditional board. I wanted to fit as many inputs, and as many types of inputs, as I could into this board, to allow for a wide variety of use cases and remap options.

# The Layout
After looking at other split keyboard options on the market, I settled on a 72-key layout, to allow for a full alphanumeric set, all modifiers, and a decent selection of macro and action keys. This mimics my current keyboard, being a 68%. I went for column stagger, as it seemed like the logical choice for a split board to me. Row staggered wouldn't make sense when tilted apart, and a grid wouldn't be optimal for variations in finger lengths across your hand. As this would result in an asymmetrical board when split on G/H, I decided to add some inputs to the left half. I placed a slide potentiometer on the bottom left, and an encoder on the top left, possibly for media controls. I placed the analog sticks next to the space keys, allowing them to be manipulated by either thumbs or index fingers easily. The Pi Pico controller fits in the top inside of each board, and a USB-C connector below for I2C crosslink. Lastly, I placed a 7-way switch above the analog stick on each side for navigation and additional bindings. 

![layout](/images/layout.png)

# The Schematic
I chose to use KiCad 9 for this project, as it is the EDA I am most familiar with now, having designed my HackPad (CineWheel editing controller) and Pixeldust (CAD controller addon) boards in it. I started by laying out a basic key matrix for the layout I designed.

![matrix](/images/matrix.png)

At the suggestion of others on the HackClub slack, I chose an MCP3008 ADC to read my analog inputs, rather than the onboard Pico ADC, as it was allegedly very bad. This communicates over SPI to the rp2040. This raised some concerns about GPIO space, as I soon realized I would not be able to connect each switch from the 7-way switch directly to the Pico. To get around this, I realized I could treat the 7-way as another row in my key matrix, so it would only take up one GPIO pin. This can be seen in the above image.

After connecting everything up and adding the other inputs, the schematic was complete. It was time to move on to the PCB.

# The PCB 
The first step of this PCB was finding the footprints for the other components. The MCP3008 and slide potentiometer were easy, but the 7-way and analog stick were a little harder. After I had those imported, I laid out the approximate layout into KiCad, on a 2-layer PCB. After I had the layout, I drew a border around everything as the shape of my board, and placed mounting holes to screw on the case and plate. I (very badly) routed all of the connections in a very rough jumble of traces that I am not proud of in any way, shape, or form. I completed the "CTRL" half of the board, but never designed the "ALT" side in this iteration, as I knew I would need to change the design.

![rev1](/images/rev1.png)

After this first attempt, I realized some major flaws in my design. Large parts of the board were being powered through single traces, there was so much via stitching (on signal traces!!!), and components such as the USB-C port I realized could be made through-hole for easier assembly. To solve many of these issues, I decided to switch to a 4-layer board, allowing me to place a solid layer of copper connected to +5V and GND. This would allow me to simply place a via wherever I needed power, rather than running a trace. This vastly simplified the routing, and allowed for easier connections to dense parts such as the USB-C port and MCP3008. 

Tonight (5/27/25) I was able to finish the routing of my PCB. This was much easier than the first revision, as power and ground were just a via away, and there was significantly less traces to route. I planned my routing with more foresight than before, choosing paths that would allow for the minimum amount of vias, especially on analog and bus traces. I drew fill zones on the inner layers connected to +5V and GND to finish out my board, and put some (decently prominent) branding on each half. I ran DRC for the last time, showing only silkscreen errors, so I knew I was in the clear.

![rev2](/images/rev2.png)

# The Case
For the case design, I am using stacked 1/16 polycarbonate, painted on one side to diffuse LEDs. This allows me to have a rigid frame while still keeping a low profile to the desk. To achieve this, I exported my ________ layer from KiCad into Onshape, along with Edge.Cuts. I created a plate from these files, with cutouts for every switch placed. I drew a border 5mm outside of Edge.cuts, to create an extension of polycarbonate beyond the board edge.
