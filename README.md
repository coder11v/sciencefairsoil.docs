<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTg6Nh39TaA23wj6dofqdoUU1Uvxvpvm6r7Pm4QQtjaVurlkjDNvUo-bopjApjaoEJp9nQcuL7pbxFh/pubchart?oid=319598435&amp;format=interactive"></iframe>

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTg6Nh39TaA23wj6dofqdoUU1Uvxvpvm6r7Pm4QQtjaVurlkjDNvUo-bopjApjaoEJp9nQcuL7pbxFh/pubchart?oid=1677544668&amp;format=interactive"></iframe>

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vTg6Nh39TaA23wj6dofqdoUU1Uvxvpvm6r7Pm4QQtjaVurlkjDNvUo-bopjApjaoEJp9nQcuL7pbxFh/pubhtml?gid=381234246&amp;single=true&amp;widget=true&amp;headers=false"></iframe>

<!--
## Precision Agriculture: Comparing Sensor-Driven vs. Timer-Based Irrigation Systems

### 1. Research Question

Does a soil moisture sensor-based irrigation system conserve water while maintaining plant health compared to a fixed-timer irrigation system?

### 2. Hypothesis

The soil moisture sensor system will save about 20% of water compared to the traditional fixed-timer system because it will optimize soil moisture and prevent the water wastage caused by over-watering.

### 3. Materials &amp; Tools

- Computing &amp; Sensors:


- 1x Raspberry Pi (ideally a Model 4 B+ or 5)
- 2x Capacitive Soil Moisture Sensors
- 1x Micro SD Card (16GB or larger)
- Breadboard and jumper wires


- Actuators (Action):


- 2x 5V Mini Water Pumps
- 2x 5V Relays (or a 2-channel relay module) to safely control the pumps
- 10-15 feet of Vinyl Tubing
- 2x 5-way Tubing Splitters (barbed connectors)
- Drip irrigation system


- Plants:


- 12x identical starter plants (I used snap peas/Pisum sativum var. Macrocarpon ser. cv.)
- 1x large bag of identical potting soil


- Constants &amp; Measurement:


- 6x identical planting pots
- 1x full-spectrum LED Grow Light
- 1x water reservoir (e.g., 2-liter bottle or small tub)
- 1x Digital Scale (with 0.01g precision, for weighing biomass)
- Graduated cylinder or measuring cup (for logging water refills)


- Software:


- Raspberry Pi OS
- Python 3
- Python library: RPi.GPIO
- GitHub
- Raspberry Pi Connect
- Filesystem for Pi
- Optional: Self-hosted data visualizer + dashboard


- HTML
- Hosting support

### 4. Variables and Experimental Design

##### Independent Variable

The type of irrigation system: Smart (soil-moisture-responsive) vs. Fixed-schedule (time-based)

##### Dependent Variables

- Primary: Final dry plant biomass (grams) after 28 days
- Secondary: Total water consumed by each system (mL)

##### Controlled Variables (held constant across both systems)

- Plant species, age, and initial size
- Potting soil type, mass, and composition
- Container size and type
- Ambient temperature and relative humidity (maintained as constant as possible)
- Grow light intensity, spectrum, and distance
- Duration of experimental period (2 weeks)
- Initial water reservoir volume

##### Controlled Variables (held constant only across each system)

- Water added

### 5. Procedure

#### 5.1. Research and Preparation

##### Part 1: Background Research

Before beginning any experimental work, conduct thorough background research on the following topics:

- Review the mechanisms of plant growth, including photosynthesis and biomass accumulation
- Research the relationship between soil moisture, nutrient availability, and plant productivity
- Understand how soil, light, water, fertilizer, temperature, and other factors impact plant health.
- Pick a few fast-growing, non-woody plants perfect for a greenhouse (think lettuce, spinach, basil, or maybe some bean sprouts)
- Note how long they usually take to grow, how much water they need, and what kind of environment makes them happy
- Figure out how big your chosen plant should get and what its dried weight might be when it's fully matured or at the end of the experiment.


- Research soil moisture sensor technology, calibration procedures, and sources of measurement error
- Investigate irrigation pump specifications, flow rates, and reliability
- Review literature on optimal grow light wavelengths, intensity (measured in μmol·m⁻²·s⁻¹), and placement distance


- Review concepts of control groups, independent variables, and dependent variables
- Understand replication requirements and statistical validity for plant experiments
- Research proper data logging protocols and CSV file standards for scientific data

##### Part 2: Materials, Plants, and Software Prep

- Create code to run on the Raspberry Pi
- Simulate an experiment with randomized integers that are in accurately predicted ranges to test the code
- Verify all electronic components via testing functionality before assembly


- Connect all electronic components according to the wiring guide
- Perform a dry-run test of the complete system before introducing water
- Verify pump activation, water flow rate, and system response to sensor input


- Set up a grow light apparatus at a stable, measured distance from the growing area
- Calibrate all sensors using known standards
- Document the specifications of your grow light (wattage, spectrum, measured intensity at plant level)


- Weigh out identical masses of soil for each pot (record the target mass and verification weights)
- Obtain 6 identical containers (pots) with drainage holes
- Label all containers clearly and consistently (A1–A3 for Smart system; B1–B3 for Fixed-schedule system)
- Repot plants with care
- Install trellises if applicable and recommended from background research.


- Set up the data storage directory on the Raspberry Pi with appropriate file permissions
- Test CSV file creation and writing functionality
- Create backup protocols for data protection (e.g., cloud backup or external drive)
- Prepare a laboratory CSV for manual observations and anomalies

* * *

#### 5.2. Experiment

1. Day 1, Start: Fill both water reservoirs with exactly 500 mL. Record volume. Place Soil Sensor A in pot A3, Soil Sensor B in pot B3.
2. Start Python script. Verify it's logging data to CSV.
3. Daily: Check water levels. When the reservoir drops below 20%, measure the exact refill volume (projected 473.176mL/wk) and log it in a notebook and as a CSV annotation.
4. Daily: Record plant observations (height, color, stress). Check the script is running.
5. Day 14, End: Stop script at recorded time. Make a final data backup.

#### 5.3. Harvest &amp; Measure

1. Cut all 10 plants at the soil line on Day 15. Place on labeled plates.
2. Rinse gently, pat dry.
3. Dry in oven at 70°C for 48 hours until brittle.
4. Weigh each dried plant to 0.01 g.
5. Take all water data and add mL values, subtract the current reservoir value.

### 6. Real-World Application

According to Khokhar (2017), agriculture uses 70% of world freshwater, and by 2050, it is estimated that that number will increase to 80.5%. Therefore, the world is in a race to grow more food with less water. The problem is that much of the irrigation is inefficient, watering with a fixed-time or estimated schedule. The project will prove that moisture sensors can be used to conserve this water without sacrificing the plant’s health.

### 7. Safety &amp; Ethics

- Safety: All electronics are low-voltage (5V), minimizing electrical risk. Water reservoirs must be kept separate from the Raspberry Pi and power outlets. Use oven mitts when handling hot trays from the oven during the drying phase. Wash hands after handling soil.
- Ethics: This project is ethically sound. The data collected is non-personal. Its goal is sustainability and efficiency.

### 8. Conclusion

My hypothesis was supported. The "smart" sensor-based watering system (Group A) produced plants with \[XX]% more biomass than the "dumb" timer-based system (Group B), while also using \[XX]% less water. The data logs clearly show the timer-based system subjected its plants to a "boom-and-bust" cycle of stress. Furthermore, the experimental data was successfully used to train a predictive AI model that could forecast final plant biomass based on sensor readings. This proves that sensor-driven systems are not only more efficient but also create a "Digital Twin" of a farm, allowing for powerful new methods of crop forecasting and optimization.

### Notes 1. Cons

#### ✅ 1. Sensor variability/inaccuracy

Problem: Low-cost capacitive soil moisture sensors can drift over time, vary by batch, or be affected by soil composition and temperature.

Why it matters: If one sensor reads “moist” differently than another, it could bias the watering frequency.

How to address it: Calibrate both sensors before the experiment (e.g., measure sensor readings in fully wet, half-wet, and dry soil). Include this in your report as a “Calibration Step.” That turns a weakness into evidence of rigor.

* * *

#### ⚠️ 2. Small sample size

Problem: You’re testing 10 plants (5 per condition). That’s not a lot, statistically speaking. Random differences between plants might affect your averages.

Why it matters: A small sample can make results less reliable or make it harder to show significant differences.

Fix / Mitigation: Acknowledge it directly in your discussion:

“Due to space and time constraints, the sample size was limited to 5 plants per group. Future work could increase the sample size to improve statistical confidence.”

* * *

#### ⚠️ 3. Controlled vs. Real-World Conditions

Problem: A grow-light setup in a room doesn’t fully mimic outdoor farming (wind, sunlight spectrum, natural humidity, etc.).

Why it matters: Your conclusions may not generalize to real agriculture.

Fix / Mitigation: Say this in your conclusion:

“While this experiment was conducted in a controlled environment, real-world validation under outdoor conditions would be the next step.”

* * *

#### ✅ 4. Time and reliability

Problem: Running a 2-week experiment with electronics, pumps, and water means possible failures (power loss, pump clogging, leaks, or SD corruption).

Fix: Add safety/reliability features like:

- Auto-saving every log entry (use flush() after each CSV write).
- Backup power (small UPS or battery).
- Manual daily status checks.

And mention:

“System uptime was manually checked daily to ensure consistent operation.”

* * *

### Notes 2. Feasibility

This is estimated to cost about $225 for materials, and it will take 2-3 months to complete. No transportation is necessary.

### Notes 3. References

- Will - [ACE Hardware](https://www.google.com/url?q=https%3A%2F%2Fwww.acehardware.com%2F&sa=D&source=editors&ust=1767938153615601&usg=AOvVaw3D8Z4zsOKzA4j6gu0rcY8H)
- People - [Bokay](https://www.google.com/url?q=https%3A%2F%2Fwww.bokaynursery.net%2F&sa=D&source=editors&ust=1767938153615916&usg=AOvVaw2mZssntcrrLuDLZ8roVFDc)
- Dadu &amp; Dadi
- Texas A&amp;M AgriLife Extension - [Easy Gardening: Sugar Snap Peas](https://www.google.com/url?q=https%3A%2F%2Faggie-horticulture.tamu.edu%2Fwp-content%2Fuploads%2Fsites%2F10%2F2010%2F10%2FEHT-015-Easy-Gardening-Sugar-Snap-Peas.pdf&sa=D&source=editors&ust=1767938153616497&usg=AOvVaw2jfHV2okIURZ9v7fKwSPzJ)
- The Spruce - [Snow Peas Growing Guide](https://www.google.com/url?q=https%3A%2F%2Fwww.thespruce.com%2Fsnow-peas-growing-guide-7482075&sa=D&source=editors&ust=1767938153616727&usg=AOvVaw3T418apNWBKHUgDsBnHXxs)
- The Old Farmer’s Almanac - [Peas](https://www.google.com/url?q=https%3A%2F%2Fwww.almanac.com%2Fplant%2Fpeas&sa=D&source=editors&ust=1767938153616888&usg=AOvVaw1U7G7X8bQvk1CC7Ta8aFRN)
- Scotts Miracle-Gro - [How to Plant Almost Anything](https://www.google.com/url?q=https%3A%2F%2Fscottsmiraclegro.com%2Fen-us%2Flearn%2Fgardening%2Fhow-to-plant-almost-anything.html&sa=D&source=editors&ust=1767938153617200&usg=AOvVaw1DpOduvlRII9jWW-yxsa2U)
- Kellogg Garden Organics - [Blue Ribbon Blend SDS](https://www.google.com/url?q=https%3A%2F%2Fwww.kellogggarden.com%2Fwp-content%2Fuploads%2F2020%2F01%2FSDS_GBO_Blue_Ribbon_Blend-1.pdf&sa=D&source=editors&ust=1767938153617495&usg=AOvVaw2dAz9blqbzVhOmeLl1azAl)
- Kellogg Garden Organics - [Blue Ribbon Potting Soil](https://www.google.com/url?q=https%3A%2F%2Fkellogggarden.com%2Fproducts%2Fgborganics%2Fgb-organics-blue-ribbon-potting-soil%2F&sa=D&source=editors&ust=1767938153617742&usg=AOvVaw1QhaLf7aoRBCvSxUI2KVAg)
- Khokhar, T. (2017) - [70% of Freshwater Used for Agriculture](https://www.google.com/url?q=https%3A%2F%2Fblogs.worldbank.org%2Fen%2Fopendata%2Fchart-globally-70-freshwater-used-agriculture&sa=D&source=editors&ust=1767938153618015&usg=AOvVaw3ubIuMPYucFG7A0Dv-joS8)
- The Raspberry Pi Official Magazine (MagPi) - [Website](https://www.google.com/url?q=https%3A%2F%2Fmagazine.raspberrypi.com%2F&sa=D&source=editors&ust=1767938153618206&usg=AOvVaw3mm12vyaay150fyW_UYNh1) and [GitHub](https://www.google.com/url?q=https%3A%2F%2Fgithub.com%2Fthemagpimag&sa=D&source=editors&ust=1767938153618299&usg=AOvVaw2eNaYovuyslDEW0YjSPOgw)
- sprinklR - Mark Niemann-Ross - [GitHub Repository](https://www.google.com/url?q=https%3A%2F%2Fgithub.com%2Fmnr%2FsprinklR&sa=D&source=editors&ust=1767938153618468&usg=AOvVaw29IY3LkJLubpqw05ut9QEg)
- Others:

* * *

- [https://sweetpeagardens.com/blogs/news/sweet-pea-obelisk-guide-securing-training-vines](https://www.google.com/url?q=https%3A%2F%2Fsweetpeagardens.com%2Fblogs%2Fnews%2Fsweet-pea-obelisk-guide-securing-training-vines&sa=D&source=editors&ust=1767938153618848&usg=AOvVaw1EYrF0buu8Xj6vpAye0rs3)
- [https://pfaf.org/user/Plant.aspx?LatinName=Pisum+sativum+macrocarpon](https://www.google.com/url?q=https%3A%2F%2Fpfaf.org%2Fuser%2FPlant.aspx%3FLatinName%3DPisum%2Bsativum%2Bmacrocarpon&sa=D&source=editors&ust=1767938153619139&usg=AOvVaw3Trb2v7kqnjPFP2wCHqoZT)
- [https://www.saferbrand.com/articles/plant-growth-stages](https://www.google.com/url?q=https%3A%2F%2Fwww.saferbrand.com%2Farticles%2Fplant-growth-stages&sa=D&source=editors&ust=1767938153619470&usg=AOvVaw2PzIXSPheuxzaL2MY4Rsfz)
- [https://www.acurite.com/blogs/acurite-in-your-home/soil-moisture-guide-for-plants-and-vegetables](https://www.google.com/url?q=https%3A%2F%2Fwww.acurite.com%2Fblogs%2Facurite-in-your-home%2Fsoil-moisture-guide-for-plants-and-vegetables&sa=D&source=editors&ust=1767938153619767&usg=AOvVaw2eZVubi8BbSw4vfnJAitWa)
- [https://portlandnursery.com/docs/veggies/peas.pdf](https://www.google.com/url?q=https%3A%2F%2Fportlandnursery.com%2Fdocs%2Fveggies%2Fpeas.pdf&sa=D&source=editors&ust=1767938153619933&usg=AOvVaw3P-CPmtDze6iQlQACO3cFO)
- [https://plants.usda.gov/DocumentLibrary/plantguide/pdf/pg\_pisa6.pdf](https://www.google.com/url?q=https%3A%2F%2Fplants.usda.gov%2FDocumentLibrary%2Fplantguide%2Fpdf%2Fpg_pisa6.pdf&sa=D&source=editors&ust=1767938153620169&usg=AOvVaw0l7G6TjbuEC_bX6JqyY-yH)
- [https://arc.net/l/quote/wdabkcls](https://www.google.com/url?q=https%3A%2F%2Farc.net%2Fl%2Fquote%2Fwdabkcls&sa=D&source=editors&ust=1767938153620393&usg=AOvVaw0FB0maXgiI4Wlu8xolLzCG)
- [https://youtu.be/hM5uux8keUU](https://www.google.com/url?q=https%3A%2F%2Fyoutu.be%2FhM5uux8keUU&sa=D&source=editors&ust=1767938153620634&usg=AOvVaw1fETMofUZZ_TjlKfVJ5dHc)
- [https://tempest.earth/resources/how-moist-should-soil-be/#:~:text=Why%20Does%20Knowing%20The%20Soil,more%20difficult%20to%20absorb%20water.](https://www.google.com/url?q=https%3A%2F%2Ftempest.earth%2Fresources%2Fhow-moist-should-soil-be%2F%23%3A~%3Atext%3DWhy%2520Does%2520Knowing%2520The%2520Soil%2Cmore%2520difficult%2520to%2520absorb%2520water.&sa=D&source=editors&ust=1767938153620972&usg=AOvVaw2M_AMJ11QFxLSwLlP6Vybc)
- [https://www.holmesseed.com/super-sugar-snap/](https://www.google.com/url?q=https%3A%2F%2Fwww.holmesseed.com%2Fsuper-sugar-snap%2F&sa=D&source=editors&ust=1767938153621137&usg=AOvVaw1CaRJOMFuQpyIuGbdMhYKB)

### Notes 4. Other

Understanding the cycle of water for the given plant is important because then we can understand how many times it will need to be watered per week, and divide the 2 cups by that number, and have an amount of water that needs to be going in every time that the soil moisture is low


Address the following in your interpretation:

- Whether the results support or refute the og hypothesis
- Biological explanation for observed differences in plant biomass
- How irrigation timing and soil moisture affected plant growth
- Efficiency gains or losses of the Smart system compared to the Fixed-schedule system
- Sources of experimental error and uncertainty (e.g., sensor calibration error, spatial variation in light, initial plant variability)
- Limitations of the experimental design and replication level
- Suggestions for future improvements or expanded studies (e.g. hydroponics)

"Agriculture accounts for over 75% of the world's freshwater consumption, making efficient water management in this sector essential." 

One of the primary challenges impeding the integration of such systems is the lack of cost-effective and reliable data-monitoring solutions.

IN REBUTTLE TO magpi

WEATHER-BASED SYSTEMS ARE FAILING

"The results highlight a potential overestimation of water requirements when using the FAO56 approach... FAO56 models tend to overestimate the ETc, especially in semi-arid climates." — Abdelmoneim et al., 2025

Translation: Traditional timer/weather systems waste water. Your sensor system is smarter.

— [Abdelmoneim](https://www.google.com/url?q=https%3A%2F%2Fwww.mdpi.com%2F1424-8220%2F25%2F5%2F1568&sa=D&source=editors&ust=1767938153623695&usg=AOvVaw03ZNy_NgJd1V3rlpq7XSWq) et al., 2025

Deficient irrigation reduced seed yield more than excessive irrigation, whereas excessive irrigation caused the greatest reduction of seed quality.

Pea yields often are increased by irrigation during

vegetative and reproductive growth, when soil moisture is otherwise limiting

— [https://www.pjlss.edu.pk/pdf\_files/2011\_2/159-164.pdf](https://www.google.com/url?q=https%3A%2F%2Fwww.pjlss.edu.pk%2Fpdf_files%2F2011_2%2F159-164.pdf&sa=D&source=editors&ust=1767938153624443&usg=AOvVaw2BKyDXmON1HhASvlC5MpwA)

## ACCURACY IS PROVEN

"The calibration results demonstrated a strong correlation between sensor readings and soil volumetric water content (θV) with an average determination coefficient (R²) of 0.967 and a root-mean-square error (RMSE) of 0.014." — Okasha et al., 2021

This is PROFESSIONAL-GRADE accuracy from a DIY sensor.

— [https://www.mdpi.com/1424-8220/21/16/5387](https://www.google.com/url?q=https%3A%2F%2Fwww.mdpi.com%2F1424-8220%2F21%2F16%2F5387&sa=D&source=editors&ust=1767938153625131&usg=AOvVaw0c99x0f-Q_9Wfad629Cf0A)

Opening paragraph: "Agriculture uses 70% of the world's freshwater (K. et al., 2022). By 2050, this rises to 80.5%. The problem? Most farms still use timer-based irrigation that either wastes water through over-watering or stresses plants by under-watering."

Middle paragraph (The Gap): "Research shows that 'water stress during flowering and seed formation is a major factor in reducing seed yields' (Martin &amp; Jamieson, 1996). Yet we know that 'real-time soil moisture-based irrigation achieves maximum yield and water-use efficiency' (K. et al., 2022). The gap? Professional sensor systems are expensive. This project tests whether affordable sensors can close that gap."

Your Hypothesis: "I hypothesize that a low-cost capacitive soil moisture sensor system will achieve the same water savings and yield benefits as expensive professional systems."



If our hypothesis is going to be proven wrong, in my conclusion, I may state that the error in the sensors was that it was not passing the threshold because it was using the soil stuck right next to it, and that soil rarely changed. And so for the further experimentation section, maybe I should put a mixture of weight sensors and soil moisture sensors and other sensors for the actuators slash pumps to work correctly.

 

URL: https://docs.google.com/document/d/1NZNKHs_ADtNgWesGKjPem8BM7BPzyfmBtljQd86CVB4/edit?tab=t.0#heading=h.8inudxc37awl
-->
