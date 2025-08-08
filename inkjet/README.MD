_https://chat.com/ ChatGPT GPT-5_

## Printers? quick overview, history and types

- Telegraph (Morse code)
- Teletypewriter
- Dot-matrix
- Laser & Inkjet

## Is the entire consumer inkjet printer market hostile?

- Razor-and-blades business model
- Planned obsolescence
- Firmware locks & DRM
- Wasted ink by design
- Ink dries out by design

## I have a consumer inkjet printer. give me tips for dealing with the cartridges

1. Print a colored test page every 14 days. If not used _~ for three months ~_ cartridges dry out and are shown as "empty".
2. Ignore "low ink" warnings, print until the actual quality drops.
3. Disable rebooting/shutdowns in printer settings, which wastes ink on "cleaning".
4. Don't take out or swap cartridges, the firmware can lock them.
5. (when purchased in the EU) Disable "Economy" or "Draft" modes, in the printer settings and the driver on windows.

## Cartridges are so expensive, can i buy non-official cartridges?

1. Buy third-party or refilled colors & black cartridges for your **printer model**.
   - Amazon
   - eBay
2. Block the printer from **internet access**, but not LAN (WiFi printing), in your home router. _This prevents the vendor from eventually disabling those cartridges with a "firmware update"._

## Here's what i got

| Product | Model                        |
| ------- | ---------------------------- |
| Printer | HP OfficeJet Pro 9010 series |
| System  | Windows 11 Pro               |
| Router  | FRITZ!Box 6660 Cable         |

## Let's do it

1. purchased on Amazon
    > Kuva 963xl Multipack Kompatibel mit HP 963 963XL Kompatibel für HP Officejet Pro 9010 9012 9013 9014 9015 9016 9018 9019 9020 9022 9023 9025 (Schwarz Cyan Magenta Gelb)
2. Disabled "HP Drucker" in http://fritz.box device list.
3. copy the `dina4.pdf` file into `USER\Documents\dina4.pdf`.
4. Install a PDF viewer and register a print job that runs every two weeks
    ```ps1
    winget.exe install -e --id SumatraPDF.SumatraPDF
    $action = New-ScheduledTaskAction -Execute "$env:LOCALAPPDATA\SumatraPDF\SumatraPDF.exe" -Argument "-print-to-default -exit-on-print `"$env:USERPROFILE\Documents\dina4.pdf`""
    $trigger = New-ScheduledTaskTrigger -Weekly -WeeksInterval 2 -DaysOfWeek Monday -At 09:00
    Register-ScheduledTask -TaskName "KeepPrinterWetToBeGood" -Action $action -Trigger $trigger -Description "Prints dina4.pdf every two weeks, so the cartridges dont dry out and report as empty" -User $env:USERNAME
    ```
