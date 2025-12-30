US Treasury Yield Curve "Fly" Monitor 🦋

📖 Project Overview

This repository contains a quantitative finance tool designed to model and monitor the relative value of the 5-Year US Treasury Note against a synthetic "Fair Value" curve derived from the surrounding maturities (the "Wings").

In fixed income trading, this is commonly referred to as a "Fly" (Butterfly) Model. The tool identifies mean-reversion trading opportunities by calculating the spread between the actual market yield of the 5-Year note and a theoretical yield implied by a Cubic Spline fit of the 3-Month, 2-Year, 10-Year, and 30-Year points.

The primary goal is to generate Rich/Cheap signals:

Rich: The 5Y yield is too low relative to the curve (Signal to Pay Rates/Sell Bonds).

Cheap: The 5Y yield is too high relative to the curve (Signal to Receive Rates/Buy Bonds).

⚙️ How It Works

The model operates on the principle that the US Treasury yield curve generally maintains a smooth, continuous shape. Dislocations in the "belly" of the curve (the 5-Year point) relative to the "wings" often present statistical arbitrage opportunities.

1. Curve Fitting (Cubic Spline)

For every trading day in the dataset, the script fits a Cubic Spline through four "anchor" points on the curve. This creates a smooth mathematical function representing the yield curve for that specific day.

Anchors: 3-Month (USGG3M), 2-Year (USGG2YR), 10-Year (USGG10YR), and 30-Year (USGG30YR).

2. Fair Value Calculation

Using the spline generated from the anchors, the model interpolates the theoretical "Fair Value" yield at the exact 5-Year maturity point.

3. Spread & Z-Score Analysis

Spread: Calculated as Actual 5Y Yield - Fair Value 5Y Yield.

Normalization: To make the spread tradeable across different volatility regimes, the script calculates a Rolling 126-Day (6-Month) Z-Score.

Signal Generation:

Z-Score > +2.0 (Red Zone): The 5Y yield is statistically "Cheap" (Yield is 2 sigmas higher than fair value). Signal: Receive Rates.

Z-Score < -2.0 (Green Zone): The 5Y yield is statistically "Rich" (Yield is 2 sigmas lower than fair value). Signal: Pay Rates.

🛠 Dependencies

To run this notebook, you will need a Python environment with the following libraries installed:

pip install pandas numpy scipy matplotlib seaborn


pandas & numpy: Used for data manipulation, time-series handling, and vector calculations.

scipy: Specifically scipy.interpolate.CubicSpline for the curve fitting logic.

matplotlib & seaborn: Used to generate the dual-axis signal dashboard.

📂 Data Requirements

The notebook requires a specific input file to function correctly. Please ensure a CSV file named Nomura Rates PCA Data.csv is present in the root directory.

The CSV must contain a Date column (or index) and the following Bloomberg ticker columns:

Column Name

Description

Role

USGG3M

US Generic Govt 3 Month Yield

Anchor (Wing)

USGG2YR

US Generic Govt 2 Year Yield

Anchor (Wing)

USGG5YR

US Generic Govt 5 Year Yield

Target (Belly)

USGG10YR

US Generic Govt 10 Year Yield

Anchor (Wing)

USGG30YR

US Generic Govt 30 Year Yield

Anchor (Wing)

🚀 Usage

Setup: Clone the repository and install dependencies.

Data: Place your Nomura Rates PCA Data.csv file in the project folder.

Run: Launch Jupyter Notebook and execute Fly Monitor.ipynb.

jupyter notebook "Fly Monitor.ipynb"


Execute All Cells: The notebook will process the history, calculate the spreads, and generate the signal chart.

📊 Output Visualization

The tool outputs a high-resolution visualization (Sniper_Rates_Fly...png) containing two panels:

Top Panel (Yields): * Displays the Actual 5Y Yield (Blue) vs. the Fair Value Model (Orange).

Allows for visual confirmation of the model's tracking ability.

Bottom Panel (Z-Score / Signals):

Displays the rolling 6-month Z-Score of the spread.

Red Fill: Highlights regions where the Z-Score exceeds +2.0 (Sell Zone).

Green Fill: Highlights regions where the Z-Score drops below -2.0 (Buy Zone).

Black Line: Zero line (Fair Value).

⚠️ Disclaimer

This software is for educational and research purposes only. It does not constitute financial advice. Trading fixed income derivatives involves significant risk.