[:arrow_left: Back to Table of Contents](/README.md)
# Extruder Skipping
These skips will typically be wider than [:page_facing_up:pockmarks.](/articles/troubleshooting/pockmarks.md)

![](/images/troubleshooting/ExtruderSkips-4.png)\
![](/images/troubleshooting/ExtruderSkips-1.png)\
![](/images/troubleshooting/ExtruderSkips-2.png)
![](/images/troubleshooting/ExtruderSkips-3.png)

Skipping below top layer:

![](/images/troubleshooting/ExtruderSkips-5.png)

- If it occurs mainly on the first layer, ensure that you are not printing with [:page_facing_up:too much squish](/articles/first_layer_squish.md) or with too much first layer flow.
- Ensure that your filament gear tension (usually a spring tensioner screw) is not too tight or too loose.
    - Yank on the filament and keep tightening the tensioner screw until it stops slipping. Tighten it a little extra, maybe 1-2 turns.
- **Use a reverse bowden tube*** with direct drive, and ensure that there is not too much resistance coming from the spool.
    - *Reverse bowden tubes go from the direct drive extruder back to the spool (and should be fixed at the spool side), and prevent fast toolhead movements from yanking the filament.
        - Use a 3mm inner diameter tube. 1.9mm/2mm ID tubes are more restrictive.
        - Ensure that it doesn't have any kinks.
        - Ensure that your spool is not catching on anything as it rotates. 
        - If you are pulling from a dry box, try without.
        - For Voron spool holders, make sure you have the PTFE tube piece installed to lessen friction.
- For Voron direct drive toolheads, ensure that you have the short piece of PTFE tubing installed between the clockwork and the hotend. 
    - Make sure it is not too long or too short. You should trim it down until it just fits without compressing the tube.
- Ensure that there are no issues with your hotend fan.
    - Ensure that your hotend fan is running and is not stopping/starting during printing from a wiring issue.
    - Also ensure that your hotend fan is running at 100%. 
        - Some vendor githubs have the `[heater_fan hotend_fan]`'s `max_power` setting at 0.4 (40%) for some    reason.
        - Ensure that you are running it at the correct voltage.
- Ensure that your hotend thermistor is correct in your config and that you are not using temps that are too    low.
- **:warning: If you use any NTC 100K B3950 thermistors**, update Klipper to the most recent version and change all instances of `sensor_type: NTC 100K beta 3950` to `sensor_type: Generic 3950` in your config. There was a [:page_facing_up:bug](https://github.com/Klipper3d/klipper/issues/4054) causing these thermistors to be inaccurate, which was fixed with a [:page_facing_up:recent deprecation.](https://github.com/Klipper3d/klipper/pull/4859)

    - Please note that some other features have been deprecated recently too. If you have not updated Klipper in a while, please see [:page_facing_up:here](https://gist.github.com/FHeilmann/a8097b3e908e85de7255bbe6246ddfd5) for instructions on how to fix up your config for the new Klipper version. 

        - You may also need to recompile/reflash your MCUs if you get a "command format mismatch" error after updating. See [:page_facing_up:here](/articles/troubleshooting/command_format_mismatch.md).

- Ensure that your retraction distance is not too high. 
    - The default Cura profile uses a high retraction distance, as it is configured for bowden. 
    - You should generally use a maximum of 1mm for direct drive.
- With the filament latch open, try extruding by hand. **It should be easy.** \
If there is much resistance, *figure out where it is coming from:*
    - You may need to drill out the filament path in the printed parts.
    - Your nozzle may be partially clogged. 
        - See if extruded plastic is shooting out to the side instead of straight down when extruding in mid-air.
        - Unclog it using a cold pull or nozzle cleaning needles.
        - Try a new nozzle.
    - Your heatbreak may be partially clogged. 
        - Remove the nozzle, cool the hotend, and try pushing fresh filament through it. Make sure to cut off the bulged tip. *If there is resistance:*
            - Unload the filament and remove the nozzle.
            - Get access to the top of the hotend (you may need to either remove the hotend or the clockwork).
            - Shine a light through the hotend and look into the other side. See if there is any plastic stuck against the walls of the heatbreak or heatsink. *If it is obstructed*: 
                - Unplug the hotend fan.
                - Heat the hotend up to ~180C.
                - We are purposefully inducing heat creep to soften the plastic in the heatbreak.
                - Push a long, thin (<=1.8mm) allen key or similar through the top side of the hotend to push the obstruction out of the bottom.
                - **:warning: Turn off the hotend as soon as you have freed the obstruction.**
                - If you let it cook without cooling for a long time, it will eventually start to soften the printed hotend mounting.
                - **Be careful - don't burn yourself!**

- Ensure that you are using the correct `run_current` for your motor. Too high or too low can both cause skipping.
    - As a general rule, don't exceed 50-60% of the rated current of your motor as your `run_current`. *Some motors like more or less current, though*, so your best bet would be to look at the stock configs or to ask in Discord.
    - Galileo/Orbiter:
        - There is some confusion about different motor models. 
            - If you have the 20mm 1a LDO motor, try 0.65a. 
            - If you have the 17mm 1a LDO motor, try 0.35-0.4a.
- Check your extruder motor [:page_facing_up:crimps](/articles/troubleshooting/crimps.md) and wiring.
- Check the volumetric speed preview in your slicer. See if it is [:page_facing_up:high for your particular hotend](/articles/determining_max_volumetric_flow_rate.md#approximate-values). Or see [:page_facing_up:here](https://github.com/AndrewEllis93/Ellis-PIF-Profile/articles/determining_max_volumetric_flow_rate.md) to determine your maximum.
    - If you are exceeding hotend limits, try lowering your volumetric speed limit in your slicer (PS/SS) or reducing line widths / layer heights / speed (other slicers) until you are under the limit.
- Try rotating the extruder (if possible) without filament loaded. It should be easy.
- Try using a cooling mod, like the [:page_facing_up:AB-BN](https://github.com/VoronDesign/VoronUsers/tree/master/printer_mods/Badnoob/AB-BN). It optimizes hotend cooling and can help with heat creep issues.
- Try lowering your extruder motor's microstepping and disabling interpolation (and stealthchop if you have it on, which you shouldn't).
- Take out the motor, and see how powerful it feels. See if you can stop it easily with your fingers. This may indicate a bad motor or bad wiring.
## **BMG Clockwork**
- Try turning the plastic gear with your finger with the motor turned off and filament unloaded. It should be relatively easy. *If there is too much resistance:* 
    - Ensure that you have a small amount of [:page_facing_up:backlash in the plastic gear.](/articles/troubleshooting/bmg_clockwork_backlash.md)
        - If they are pushed together too hard, it will cause resistance.
    - Ensure that your drive shaft is not rubbing against the motor:
        - A little cheat I have heard here is to test continuity between the drive shaft and the motor. Test throughout the full rotation.
        - ![](/images/troubleshooting/ExtruderSkips-Clearance.png)