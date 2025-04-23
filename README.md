# PGA Tour 2K25 Distance Calculator

A clean and interactive web-based tool to help players accurately calculate the **true playing distance** and **side-to-side shot offset** in PGA Tour 2K25 based on elevation, wind, and ball lie.

## 🌐 Live Webpage

Try the calculator live here: [PGA Tour 2k25 Distance Calculator](https://pga-2k25-distance-calculator.vercel.app/)


## 🎯 Features

- Input game-relevant shot conditions: distance, wind, elevation, and slopes
- Calculates:
  - Headwind/Tailwind adjustment
  - Crosswind push
  - Elevation adjustment (feet to yards)
  - Lie and side slope effects
  - Final playing distance
  - Left/Right offset
- Clean UI with real-time feedback and error checking
- Fully client-side with HTML/CSS/JavaScript, suitable for GitHub Pages

## 📥 Inputs

| Field                    | Description                                              |
|-------------------------|----------------------------------------------------------|
| Yards to Pin            | Raw yardage from your ball to the pin                    |
| Elevation Change (feet) | Elevation difference (positive = uphill, negative = down)|
| Wind Speed (mph)        | Speed of wind from game HUD                              |
| Wind Direction (°)      | **Where the arrow is pointing** (0–359°)                 |
| Lie Slope               | `Upslope`, `Downslope`, or `Flat`                        |
| Lie Gradient (Color)    | Game-provided lie difficulty: `Green`, `Yellow`, `Red`   |
| Side Slope Direction    | Ball above or below feet: `Above feet`, `Below feet`, `Flat` |
| Side Slope Gradient     | Severity of side slope: `Green`, `Yellow`, `Red`         |

## 🧮 Calculations Explained

### Wind Direction Reference

| Degrees | Direction |
|---------|-----------|
| 0       | North     |
| 90      | East      |
| 180     | South     |
| 270     | West      |

### Wind Components

- **Input Wind Direction** is the arrow's pointing direction in-game.
- Internally converted to wind **origin** using:  
  `(windDir + 180) % 360`

#### ➤ Headwind Component:
`headwind = windSpeed * cos(toRadians(windFrom))`

#### ➤ Crosswind Component:
`crosswind = windSpeed * sin(toRadians(windFrom))`

### Wind Adjustment Table

Used in calculating how much wind helps or hurts distance.

| Wind Type   | Range        | Yards Added/Subtracted (before scaling) |
|-------------|--------------|------------------------------------------|
| Downwind    | 1–10 mph     | 1 yd/mph                                 |
|             | 11–16 mph    | +1.5 yd/mph (above 10)                   |
|             | >16 mph      | +2 yd/mph (above 16)                     |
| Into Wind   | 1–10 mph     | 1 yd/mph                                 |
|             | 11–16 mph    | +1.5 yd/mph (above 10)                   |
|             | >16 mph      | +2 yd/mph (above 16)                     |

After calculating the total yardage from wind:

- Multiply **Downwind** by **0.8**
- Multiply **Into Wind** by **1.2**

### Lie Adjustment Table

| Slope     | Wind | Green | Yellow | Red  |
|-----------|------|--------|--------|------|
| Upslope   | Into | +1     | +2     | +5   |
| Downslope | Into | 0      | -0.5   | -2   |
| Upslope   | Down | 0      | 0      | 0    |
| Downslope | Down | 0      | 0      | 0    |

### Side Slope Offset Table

| Gradient | 100 yds | 200 yds |
|----------|---------|---------|
| Green    | 1 yd    | 2 yds   |
| Yellow   | 2 yds   | 5 yds   |
| Red      | 10 yds  | 20 yds  |

If the ball is:
- **Above feet** (right-handed player): offset is **to the left (negative)**
- **Below feet**: offset is **to the right (positive)**

## 🧾 Final Calculations

### Playing Distance:
`yardsToPin + (elevationFeet / 3) + (headwind > 0 ? windAdj * 1.2 : -windAdj * 0.8) + lieAdjustment`

### Left/Right Offset:
`Math.round(crosswind + sideSlopeOffset)`

## 🚀 How to Use

1. Open `index.html` in a browser.
2. Fill in all shot conditions.
3. Click **Calculate** or press **Enter**.
4. View the computed distance and offset on the right.

You can also host it on GitHub Pages by pushing this code to a repo and enabling Pages in Settings.

## 📁 File Structure

├── index.html # Main application file <br>
├── README.md # Project description, usage, and logic <br>
└── /assets # screenshots, icons, or supporting media

## 📘 License

This project is licensed under the **GNU General Public License v3.0**.

You are free to use, modify, and distribute this software under the terms of the GPLv3. See the full license text at [gnu.org/licenses/gpl-3.0](https://www.gnu.org/licenses/gpl-3.0.html).


## 🙌 Credits

Made with ❤️ for PGA Tour 2K25 fans who want to take control of their shots.

Special shoutout to [Early1981](https://www.youtube.com/watch?v=neM23bmX3f8&t=1s) and [Ma_Kachada](https://www.youtube.com/watch?v=MTUrDbW_5DA&t=5s) for their YouTube videos which made this much easier to calculate and verify.
