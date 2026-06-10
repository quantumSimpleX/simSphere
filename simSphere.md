Here is the formatted Markdown version of your prompt. I have cleaned up the scattered source tags, structured it with a clear hierarchy, and used bolding and blockquotes to make the requirements, UI controls, and complex behavior easy to read at a glance.

---

# Technical Specification: 3D Light & Sphere Simulation

Write a single, static, responsive HTML page that simulates a 3D scene of a light gray stone sphere resting on a rectangular landscape, illuminated by four color-cycling spotlights.

---

## 📄 Core Requirements

* **Scene Setup:** A rectangular landscape with a light gray stone sphere placed exactly in the center. Four spotlights are positioned at the four corners of the landscape, shining directly toward the center sphere.
* 
**Initialization:** The browser window/canvas must initialize properly before rendering so that the sphere appears perfectly spherical right from the start (no initial distortion). 



---

## 🎛️ UI Controls & Parameters

### 1. Geometry Controls

* **Landscape Dimensions:** Width and length adjustable from `1` to `6` feet. *(Default: 4 ft × 5 ft)* 


* **Sphere Diameter:** Adjustable from `1` foot up to the length of the shorter side of the landscape. *(Default: 2 ft)* 



### 2. Global Light & Beam Controls

* **Ambient Light Level:** Range `0` to `1` (`0` = complete darkness, `1` = bright indoor daylight). *(Default: 0.1)* 


* **Spotlight Brightness:** A single global control ranging from `0.01` to `1` (`0` = completely off, `1` = 800 lumens). *(Default: 1)* 


* **Beam Angle (Spread):** Universal control for the angular spread of all spotlights, ranging from a narrow beam (`5°`) to a wide beam (`90°`). *(Default: 55°)* 


* **Angular Aim:** Universal control altering the vertical tilt from `-5°` to `45°` (`0°` = completely horizontal, `-5°` = aimed slightly at the ground, `+45°` = aimed 45 degrees upward). *(Default: 20°)* 


* **Spotlight Height:** Universal control adjusting the height of the spotlights above the landscape, represented as a single decimal percentage of the sphere's radius. `0%` means the light sits on the ground; `100%` means it matches the height of the sphere's center. *(Default: 10%)* 



### 3. Animation & Timing Controls

* **Cycle Speed:** Controls the rainbow cycling frequency in Hertz using **25 steps on a logarithmic scale**. Range: `0 Hz` (static/frozen) to `1 Hz` (1 full cycle per second). *(Default: 0.2 Hz)* 


* **Phase Delay:** Controls the delay between successive lights using **25 steps**. Range: `0 seconds` (perfectly synchronous) to `3 seconds`. *(Default: 0.3 seconds)* 


* **Pause/Resume:** Clicking the mouse on the scene or pressing the `p` key toggles between pausing and resuming the rainbow cycling.



---

## 🌈 Spotlight Color Behavior

> ⚠️ **Crucial Color Rendering Logic:** The spotlights must cycle through the colors of a realistic rainbow: **Red → Orange → Yellow → Green → Blue → Indigo → Violet**. 
> 
> 

* **RGB Pathing:** This cannot be a simple HSL color sweep. The animation must trace the specific path within the RGB color space that realistically represents a rainbow. 


* **Perceived Luminance Match:** You must hold the **perceived intensity** of the light constant. As the colors transition, there should be no sudden dimming, abrupt spikes, or perceived brightness shifts (e.g., yellow must not appear perceived brighter than blue). 


* **Seamless Loop:** When a light reaches Violet, it must smoothly blend back into Red. This loop transition should take roughly the same amount of time as it takes to move between any other two consecutive rainbow colors, creating an infinite, fluid cycle. 


* **Sequential Delay Mechanics:** The 4 lights—labeled clockwise as **A, B, C, and D**—must stagger their animation starts based on the Phase Delay setting. For example, if the delay is set to `2 seconds`:
* Light **A** starts immediately (`0s`).
* Light **B** starts `2s` later.
* Light **C** starts `4s` later.
* Light **D** starts `6s` later.


## 🌈 Spotlight Color Space Widgets

* **Color Space Path:** Add a widget on floating on top of the simulation plan showing how each light A, B, C, and D traverse through the color space.


* **RGB Color Coordinate Path:** Widget on floating on top of the simulation plan showing how each light A, B, C, and D as dots traverse through the 2D CIE chromaticity diagram.


* **Legend:** Display a legend at the bottom of the simulation telling the user they can click the mouse or press `p` to pause/resume the simulation, along with any other relevant usage notes (e.g., drag to orbit, scroll to zoom). The legend should reflect the current paused/running state.

