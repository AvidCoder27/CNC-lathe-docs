# Mach3 Software Setup

This page will describe how to install and configure the software required to run the CNC lathe.

# Requirements

Before you begin, ensure that you have a computer running **Windows 10 or 11** with a working **USB port**. You must also have internet access.

# Summary

In this section, you will:

- [Download](https://www.machsupport.com/downloads-updates/mach3-downloads/) the Mach3 installer
- Install Mach3 to a folder on your desktop
- Install select program features
- Copy a [custom config file](https://github.com/AvidCoder27/CNC-lathe-docs/raw/refs/heads/main/downloads/MachMillLathe.xml) into your installation
- Copy a [plugin](https://github.com/AvidCoder27/CNC-lathe-docs/raw/refs/heads/main/downloads/RnRMotion.dll) into your installation

# Step-by-Step Guide

## Download the installer

1.  Navigate to [Mach Mach3 Downloads](https://www.machsupport.com/downloads-updates/mach3-downloads/)
2.  Click big red button that says **DOWNLOAD MACH3**
3.  Save the file to your downloads folder

## Install Mach3

1.  **Close all programs** open on your computer.
    1. This is recommended by the installer.

2.  **Run the installer** from your downloads folder
    1. The file should be called something like **Mach3VersionX.XXX.exe** where X.XXX is the version number.

3.  It will ask for administrator **permission**; select **Yes**
4.  The Mach3 Setup program will **launch.**
5.  Select **Next.**
6.  Select **I agree to the terms of this license agreement.**
    1. You may read it if you like.

7.  Select **Next.**
8.  Now, we will choose **where to install** Mach3.
    1. The default location is normally fine, **but** for this tutorial we will install it **onto our desktop** because it will make our lives easier later.
    2. Select **Change.**
    3. Select **Desktop**, then click **Make New Folder.**
    4. Press **F2** to rename the New Folder; type **Mach3** and hit Enter.
    5. Select **OK.**
    6. The installation path should now be: **C:\\Users\\\<user\>\\Desktop\\Mach3**

9.  Select **Next.**
10. Ensure the following program features are **selected**:
    1.  Wizards
    2.  XML’s
    3.  Screen sets

11. Ensure to **deselect** the following:
    1.  LazyCam (not needed)
    2.  Parallel Port Driver (can cause problems later)

12. Select **Next.**
13. Select **Next** again.
    1.  We will skip this step for now, and import our own custom profile later.

14. Select **Next** to begin the installation.
    1.  Installation takes under two minutes on a decent internet connection.

15. Once installation is complete, hit **Finish.**

## **Customize the installation**

1.  **Download** the [MachMillLathe.xml](https://github.com/AvidCoder27/CNC-lathe-docs/raw/refs/heads/main/downloads/MachMillLathe.xml) configuration file.
    1.  Follow the link, then right click and select **Save as...** to save the file to your downloads folder.

2.  **Move** the **MachMillLathe.xml** file from your Downloads folder **into the Mach3 folder** on your desktop.

3.  **Download** the [RnRMotion.dll](https://github.com/AvidCoder27/CNC-lathe-docs/raw/refs/heads/main/downloads/RnRMotion.dll) plugin.

4.  **Move** the **RnRMotion.dll** file from your Downloads folder into the **PlugIns folder** of your **Mach3** folder. _(C:\\Users\\<user>\\Desktop\\Mach3\\PlugIns)_

## **Test the installation**

1.  **Connect** the **USB** cable from the Lathe to your computer.
2.  On your desktop, there should be a shortcut called **Mach3 Loader**; double click it
3.  Select the **MachMillLathe** profile and hit **OK** to load it.
    1. If you do not see the MachMillLathe profile, then you probably incorrectly moved the MachMillLathe.xml file. Check your Mach3 folder for the MachMillLathe.xml file.

4.  You should be prompted to choose a **control device.**
    1. Select **RnRMotionController**
    2. Select **Don’t ask me this again**
    3. Select **OK**

## **You can now use Mach3 to control the CNC Lathe!**
