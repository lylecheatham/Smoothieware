To upgrade from master or edge to the new motion control firmware
-----------------------------------------------------------------

The following changes must be made to your config

1. alpha_max, beta_max, gamma_max must be correctly defined for homing to work properly even when homing to min
   they control the maximum distance the axis will move before it gives up finding the home switch.

2. it is best to start from a fresh config

3. you must delete the config-override (M502) as M203 format has changed (M203 sets cartesian max speeds, M203.1 sets actuator max speeds, no longer uses ABC as these are now reserved for future n-axis). (or for a short while just do M500 as the  A B C will be read until it is deprecated, and M500 will save it in the new format)

4. Homing is slightly different, by default it will home X and Y axis at the same time then Z, this can be reversed and have Z home first then X and Y.
   the homing_order setting still works the same way as before.

5. The old extruder syntax is no longer allowed so check your config has the latest extruder config like
```
extruder.hotend.enable                          true             # Whether to activate the extruder module at all. All configuration is ignored if false
extruder.hotend.steps_per_mm                    710              # Steps per mm for extruder stepper
extruder.hotend.default_feed_rate               600              # Default rate ( mm/minute ) for moves where only the extruder moves
etc
```

and not the old

```
extruder_module_enable                       true
```

6. If you use volumetric extrusion (M200 D2.85) then note that unlike the current edge, G1 E5 will extrude 5mm³ not 5mm, also note that the extrude length setting in Slic3r will need to be specified in mm³ not mm unless you use firmware retraction (That retraction length is specified in M207 and is in mm).

The following changes must be made to your hardware
---------------------------------------------------

1. Due to a mistake in the previous versions of the firmware the E direction was reversed, so you must invert your dir pin for your extruders (or reverse the extruder plug) from how they were before.

