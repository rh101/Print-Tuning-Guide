[:arrow_left: Back to Table of Contents](/README.md)
# Determining Motor Currents
**:warning:** The below guidance is for **A/B/X/Y motors only**.

Extruder motors/pancake steppers are a bit different, as there is more variance between models.

- **Check with the community first.**
    - If you are using BoM motors, check the stock configs.
    - Check in Discord to see what others are running.

- **You should start off with a more conservative** `run_current`.
    - You may be able to attain additional motor performance by increasing currents, but come back to that later. **Get your printer working reliably first.**

- **Some motors vary.**
    - I have found my LDO 0.9 degree steppers to be able to achieve notably higher max accels/speeds with higher currents. 
    - My OMC 1.8 degree motors, on the other hand, performed very well even at moderate currents.

- We are derating the motors/drivers for margin of safety. Rated currents are the absolute maximum *in ideal conditions*. In reality, things like chamber and driver temperature come into play. Margin of safety is also standard practice.
- TMC2209 drivers are rated to 2a RMS, but I would not exceed 1.4a RMS.

## Determining Initial `run_current`:
Start with around **40-50%** of rated current.

For example, with a 2a motor, start around 0.8-1a.
## Determining Maximum `run_current`:
A good rule of thumb is to not exceed **70%** of the rated current.

For example, a 2a motor would be about 1.4a max.


- Keep in mind that currents approaching maximum may need greater stepper driver cooling.
- If you are pushing higher currents, you may also want to consider measuring the temperature of your motors. Ensure that they do not exceed 75-80C.
    - Measure the temps when actually printing in a heat soaked chamber.
        - Some multimeters come with a k-type thermocouple. You can kapton tape it to the motor housing.
    - *You cannot accurately gauge this by feel.* Even lower temperatures will feel "too hot".
    - The motors themselves can generally handle much more. This temp limit comes from the printed parts rather than the motors themselves.
## Determining `hold_current`
Recently, Klipper docs have started to [:page_facing_up:recommend against using a separate `hold_current`.](https://github.com/Klipper3d/klipper/pull/4977) You can achieve this by commenting out `hold_current`, or by setting it to the same value as your `run_current`.

If you run a different `hold_current`, a good rule of thumb is about 70% of your `run_current`.