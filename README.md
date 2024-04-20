# LCXLTrackDisplayFix
## The problem.
Ever noticed this when using the Track Select buttons on your LCXL?  
  
![Screenshot 2024-03-28 095434](https://github.com/Darkl0ud/LCXLTrackDisplayFix/assets/11672085/d6d98113-a7af-4363-83b5-93094124dccd)  
  
It doesn't make sense! The LCXL can control 8 tracks at a time. So how are we controlling tracks 1 - 9? In the script for the LCXL, there is a programming error where the integer is off by 1 when adding the amount of tracks the LCXL can support (and checking the max amount of tracks) to the currently controlled track of bank 1 on the LCXL. In other words, if bank 1 of the LCXL is controlling track 1 and we have more than 8 tracks, it should report "Controlling Track 1-8", but instead it reads "Controlling Track 1-9".

## The fix.
I decompiled __init__.pyc, ButtonElement.pyc, DeviceComponent.pyc, LaunchControlXL.pyc, MixerComponent.pyc, and SkinDefault.pyc, all found within Ableton's MIDI Remote Script LXCL resource folder, fixed some bytecode with Pylingual so it would give an accurate decomp, change the line:
```
end = min(start + 8, len(session.tracks_to_use()))
```
to
```
end = min(start + 7, len(session.tracks_to_use()))
```
I then plopped these uncompiled .py files into the Ableton MIDI Remote Script LCXL resource folder, opened up Ableton Live, Live compiled the scripts, then I set "LaunchControlXL" as a Control Surface in Preferences.
Not it reads the correct values.

## Install

> [!WARNING]
> I am not responsible for any damages, etc. Though, I don't see how this could break anything. That's why we make BACKUPS. This has only been tested on Live 12.
1. Close Live if it's open.
2. Backup your current LaunchControlXL.pyc from Ableton's folder for the MIDI Remote Script for the LCXL.  

Windows
```
   C:\ProgramData\Ableton\YOUR LIVE 12\Resources\MIDI Remote Scripts\Launch_Control_XL
```
Mac
```
  /Applications/YOUR LIVE 12.app/Contents/App-Resources/MIDI Remote Scripts/
```
4. Copy LaunchControlXL.pyc into the same directory.
5. Open Live.

   Your LCXL should boot up like normal and you should now see the correct messages on the bottom left of your screen when switching banks.
