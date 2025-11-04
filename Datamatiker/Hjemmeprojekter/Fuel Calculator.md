# Inspiration:

[SimRacingFuelCalc](https://pitskill.io/fuel-calculator)
   

# Main purpose:

Easily calculate the required amount of fuel for the race or stint.  
Several options for calculations should exists:

- Race or stint duration
- Amount of laps
- Different options for safe and risky amounts.
 
# Features:
 
### First iteration:

Simple local WPF app based calculator tool to do basic calculations.  
Starting out I'm only focussing on Le Mans Ultimate for assessing accuracy, this may expand.  
No persistence.
 
### Second iteration:

UI changes with images, icons, graphics and animations.  
Maybe light persistence.
 
### Third iteration:

Link to database to do avg calculations and statistics.  
Adding the database, the design needs to account for multiple games with different statistics.  
UI must be updated to suit this and also utilize the stored data.  
Both car and track info needs to be stored properly.
 
### Fourth iteration:

Look into web or mobile applications to see what's possible.
   

# Calculations:
 
## Based on race/stint time:

(RaceLengthInSeconds / LapTimeInSeconds) x ConsumptionPerLap = Result
 
## Based on laps:

RaceLaps x ConsumptionPerLap = Result
 
## Average based on track and car combo:

This will purely consist on data from the database. The logic for this still need to be figured out.