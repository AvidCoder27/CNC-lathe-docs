# Fusion360 Setup

This page will describe how to license, install, and configure Fusion360
for use with the CNC lathe.

# Requirements

Before you being, ensure that you have a computer running **Windows 11**.
If this is not the same computer that you are running Mach3 on, then you
will always need a way to transfer files between them, such as a thumb drive.

# Summary

In this section you will:

- Get a FREE educational Fusion360 license
- Download and install Fusion360
- Create a default template project, including configuring:
  - the cutting tools
  - the post-processing options

# Step-by-Step Guide

## Aquire a free Autodesk license and Install Fusion

Autodesk actually provides a guide to help students access and install Fusion for
educational use.
Follow the directions in [FusionSingleInstallInstructions.pdf](./FusionSingleInstallInstructions.pdf)

Before you read that, take note of one thing:

You will be choosing **Option 1** (you'll know what that means once you start following the pdf's directions).

Once you have completed that guide, you should have a licensed version of Fusion360 installed on your computer.

## A quick note about Fusion

Fusion360 is a very complex piece of software and sometimes you will be confused.
There are also so many options and toggles and things that can go wrong that there is
no way we can explain or predict them all in these docs.

We will do our best to explain many things, but some things you will have to figure out on your own (e.g. google it!)
If you do run into a problem, and you find a solution, **please open an issue or create a pull request to update these docs and provide guidance to anyone else who may have the same problem.**

With that said, you may want to follow some other tutorial to learn the very basics of navigating Fusion.

### TODO: Add a link to a good, basic fusion tutorial.

As a result, we will assume at least very basic knowledge of how to use Fusion360.

## Designing a template part

### TODO: Insert images to assist with CAD

1. Create a new, empty design in fusion. Set the document units to inches.

2. In the design tab, create a new sketch.
   1. Place the sketch on the front plane.
   2. Use the rectangle tool to create a rectangle with one corner at the origin.
   3. Dimension the rectangle to a height of 1 inches and a width of 0.375 inches.
   4. Create a second rectangle on top of the first rectangle but aligned to the left side.
   5. This second rectangle can also be 1 inch tall, but only 0.3 inches wide.

      _Note: these dimensions are arbitrary since we are just making a template for later.
      If you would like to get creative with the template, feel free, but just keep in mind
      that all of our stock (at the time of writing this) is 0.75 inches in diameter._

   6. Select `Finish Sketch`.

3. Use the revolve tool.
   1. Set the `Profile` to be the two rectangles from the sketch you just made.
   2. Set the `Axis` to be the vertical line emanating up from the origin.
   3. Set `Extent Type` to `Full`.
   4. Set `Operation` to `New Body`.
   5. Select `Okay`.

4. You should now have a body that looks two cylinders stacked on top of each other.
   1. Save your document with Ctrl+S
   2. Title it something like "Lathe Template" and select `Save`.
      _It is a good idea to save your document often so that you don't lose your work.
      It also makes it easier to roll back changes if you make a mistake._

## Setup and tooling for manufacturing tab

1. **Switch to the `Manufacture` tab in the upper left hand corner.**
   1. Switch to the `Turning` along the top of the window.

2. Under the `Setup` drop down (upper left corner), select **`New setup`.**
   1. Ensure `Operation Type` is `Turning or mill/turn`.

   2. Ensure `Spindle` is `Primary Spindle`.

   3. Under `Work Coordinate System (WCS)`, set `Orientation` to `Model orientation`.

   4. Ensure `Origin` is `Stock front`.

   5. Switch to the second tab in the setup: `Stock`.

   6. Ensure `Mode` is `Fixed size cylinder`.

   7. Set `Stock Diameter` to 0.75 inches.

   8. Set `Length` to 4 inches.

   9. Ensure `Model Position` is set to `Offset from front`, and set `Offset` to 0.1 inches.

   10. Change `Round Up to Nearest...` to 0 inches -- we do not want rounding.

   11. Switch to the third tab, `Post Process`.

   12. Set the `Program Name/Number` to "Template".

   13. Set the `WCS Offset` to 1. **This is very important.**

   14. Ensure `Multiple WCS Offsets` is deselected.

   15. Select `Ok` to create your setup.

   16. **Save your document** with Ctrl+S.

3. **Now, we are going to add some tools.**
   1. Under the `Manage` drop down (upper right), select `Tool Library`.

   2. On the left, navigate to `User Libraries > Documents > Lathe Template`.

   3. In the upper left corner, select the plus sign to create a new tool.

   4. Scroll down to the Turning section and select `Turning general`.

   5. In the new popup, set the description to `LHand DCGT321`.

   6. In the `Insert` tab, set the `ANSI code` to `DCGT321`.
      1. Make sure to write this code EXACTLY.

      2. The ANSI code tells Fusion the exact specifications of our tool.

   7. Ensure: `Type` is `Turning general`, `Unit` is `Inches`, and `Material` is `Carbide`.

   8. In the `Holder` tab, set the `Style` to `L = -5deg side (both)`

   9. Set `Hand` to `L = left handed` and the `Clamping` to `D = rigid lock`. The other defaults are fine.
      **TODO specify actual holder settings**

   10. Skipping `Setup`, move to the `Cutting data` tab.
       1. On the left, select the plus to create a new cutting preset; call it "Wood"

       2. Disable `Use constant surface speed`.
          1. This is because our lathe's spindle speed is not controlled by the G-Code. Instead, we set it with a knob.

          2. By turning off constant surface speed, we allow surface speed to change, meaning that spindle speed is constant.

       3. We do not want to have to change the spindle speed by hand during a cutting operation! 3. Set the `Spindle Speed` to 10000 rpm (that's 10 thousand!).

       4. Enable `Use feed per revolution`.
          1. Set `Cutting feedrate per revolution` to 0.012 inches

          2. Ensure that it automatically updates the other two feedrates below.

       5. Disable `Use depth of cut`.

       6. Set `Coolant` to `Disabled`.

   11. Switch to the `Post processor` tab. Ensure that:
       1. `Number` is 1

       2. `Break control` is disabled

       3. `Compensation offset` is 1

       4. `Manual tool change` is disabled

       5. `Turret` is Fusion360

   12. Select the blue `OK` in the lower right to create your new tool.

   13. Select the new tool, and hit Ctrl+D to duplicate it.
       1. Double click your duplicated tool and change the following settings:
          1. Change the description to `RHand DCGT321`

          2. Under the `Holder` tab, set `Hand` to `R = right handed`.

       2. This will create a the version of the tool for making so-called "right-hand" cuts.
          The cutting bits are identical, the only difference is how they are held and how they interact with the stock.

       3. Select `OK` to confirm your new, right hand tool. 14. Hit Escape to exit the tool manager.

## Creating toolpaths

We will now create some toolpaths. Toolpaths have **a lot** of options, so if an option is not explicitly mentioned, just leave it as default.

1. First, we will make a **facing operation** to remove the stock at the tip.
   1. Under the `Turning` drop down, select `Turning Face`.
   2. Set the tool to be `#1 - DCGT321 LHand DCGT321`.
   3. Scroll down to the "Feed & Speed" section and select the `Wood` preset that you just created.
   4. Switch to the Radii tab (the third tab from the left)
      1. Under the `Clearance` section, set the `Offset` to be 0.1 inches
      1. Under the `Retract` section, set the `Offset` to be 0.1 inches
   5. Switch to the Passes tab (the fourth tab from the left)
      1. Set the `Direction` to `Outside to inside`
      2. Under the Passes section, enable `Multiple Passes`
         1. Make sure the `Stepover` is at most 0.04 inches.
         2. Stepover is the max distance that it's allowed to cut away at a time.
         3. This will force the machine to make multiple cuts to face the stock.
   6. Select OK
   7. Fusion will take a couple seconds to generate a toolpath.

2. Now, we will create a **roughing operation.**
   1. Under the `Turning` drop down, select `Turning Profile Roughing`.
   2. Set the tool to be `#1 - DCGT321 LHand DCGT321`.
   3. Scroll down to the "Feed & Speed" section and select the `Wood` preset that you just created.
   4. Switch to the Radii tab (the third tab from the left)
      1. Under the `Clearance` section, set the `Offset` to be 0.1 inches
   5. Switch to the Passes tab (the fourth tab from the left)
      1. Set the `Direction` to `Back to front`

3. Next, we will make a **finishing operation.**
   1. Under the `Turning` drop down, select `Turning Profile Finishing`.
   2. Set the tool to be `#1 - DCGT321 LHand DCGT321`.
   3. Scroll down to the "Feed & Speed" section and select the `Wood` preset that you just created.
   4. Switch to the Radii tab (the third tab from the left)
      1. Under the `Clearance` section, set the `Offset` to be 0.1 inches
   5. Switch to the Passes tab (the fourth tab from the left)
      1. Set the `Direction` to `Back to front`

## Simulation

We have created our three toolpaths!
Now, we will simulate them to check what they will look like and to check for errors.

1.  In the left pane (called the hierarcy), select your Setup (likely called `Setup1`).

2.  In the upper bar under actions, select `Simulate`.

3.  Now you are in the simulation view.
    _This is a very useful and powerful tool with a few settings.
    Play around with a few of them to see what they do.
    **It is recommended to turn on `Regenerate on rewind` under the Stock section**_
4.  Ensure that there is not a pop-up in the upper right hand corner warning of errors.
    _If there is, you have likely done something wrong. You are encouraged to do a google search on the error code and figure it out yourself._

5.  Hit the play button at the bottom of the screen (or hit Ctrl+Space) to simulate all the operations.

6.  You should see it remove the tip of the stock, then run a roughing operation that removes most of the stock, and then a finishing operation to make it nice.

7.  Once you are done, hit the check mark at the top labeled `Exit Simulation`

## Generating NC code

Great! We have our toolpaths! The final step of the Fusion process is to post to g-code.
This is essentially exporting our toolpaths to a file that the lathe can read and execute.

1.  First, you must download and import the Post configuration file.
    1. Download the file [mach3turning.cps](https://github.com/AvidCoder27/CNC-lathe-docs/raw/refs/heads/main/downloads/mach3turning.cps).
       _This is the configuration for your posts._

    2. Hit Windows+R on your keyboard to open the windows run prompt, and type `%appdata%`.
       This will open file explorer.
    3. Open the "Autodesk" folder, and then "Fusion 360 CAM", and then into "Posts".

    4. Copy the `mach3turning.cps` file from your downloads into the Posts folder.
       This will make it available in Fusion.

2.  Now, switch back to Fusion where we will create a new NC Program.
    1. At the top under the `Setup` dropdown, select `Create NC Program`.

    2. Under the `Post` dropdown, select `Mach3 Turning for Boxford Dueti Turn & Mill CNC Lathe`
       _If you do not see this option, try canceling the popup and try again.
       If it still doesn't work, you likely failed to properly copy the mach3turning.cps file into
       your posts folder._

    3. Ensure that `Unit` is set to `Millimeters`.

    4. Choose an `Output folder` that suits your needs.
       _If you are using the same computer for Fusion and Mach3, then create a folder on your
       computer for your G-code files. If you are using separate computers and need to transfer
       the g-code to another computer, you can select a folder on a thumb drive._

    5. Select `Post` to create your NC file.

    6. Fusion should say in the lower right: "NC code successfully posted"
       _If it comes up with an error, do a google search! You got this!_

    7. The NC code file will be saved in the location you specified with the name `template.nc`.

### You should now have a file that you can run on the lathe!
