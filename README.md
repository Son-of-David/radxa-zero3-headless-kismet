# radxa-zero3-headless-kismet
Run kismet on a radxa-zero-3 with your phone and a wireless adapter. Pipe your phones gps data to kismet for precise coordinates of information gathered. 

# S.O.D – Radxa Zero 3 Kismet Headless AP + Phone GPS Bridge

This project showcases a Radxa Zero 3 configured to:
- Run Kismet headlessly using an external Wi‑Fi adapter (tested: PandaWireless PAU0F AXE3000, ALFA AWUS036AXM).
- Host an access point for mobile viewing and control.
- Bridge phone GPS data (via the Android app gpsdRelay) into Kismet using a UDP→gpsd socat pipe.
- Provide a simple web control panel on the device (SOD Control Panel).
- Optional: A Waveshare 1.3" LCD Top Hat (ST7789) UI with a basic menu and mini games (not required for core functionality).

## Gzipped 16GB SD card image available for download at 

https://torchurch.us

## Notes:

- The AP comes up at boot on 14.7.8.1 with SSID “S.O.D‑Link” and password “S.O.D‑MI7MI8”.
- The SOD Control Panel is at http://14.7.8.1:478/
- Once Kismet is launched, its web UI is at http://14.7.8.1:2501/ (Username: SOD, Password: MI7MI8).
- Phone GPS can be sent via gpsdRelay (F-Droid) using UDP IPv4 to 14.7.8.1:9999.
- Maintenance updates are easy via SSH with an external Wi‑Fi adapter (or HDMI + keyboard) and the teardown script.

## Features

- Headless Kismet runner that works with external USB Wi‑Fi adapters:
  - Tested: PandaWireless PAU0F AXE3000, ALFA AWUS036AXM.
- AP on boot:
  - IP: 14.7.8.1/24
  - SSID: S.O.D‑Link
  - Password: S.O.D‑MI7MI8
- SOD Control Panel (web):
  - http://14.7.8.1:478
  - Start/stop Kismet, view Kismet terminal messages, GPS snapshot, Wi‑Fi client mode.
- Kismet web UI:
  - http://14.7.8.1:2501 (Username: SOD / Password: MI7MI8)
- Phone GPS bridge:
  - gpsdRelay (from F‑Droid) → UDP to 14.7.8.1:9999 → socat PTY → gpsd → Kismet.
- Optional LCD UI and mini games with a Waveshare 1.3" ST7789 top hat.

## Hardware

- Radxa Zero 3
- External Wi‑Fi adapter (recommended tested models above)
- Optional: Waveshare 1.3" LCD hat (ST7789)
- Optional: U‑blox 7 USB GPS module (works; phone GPS is more convenient)

## Network and Access

- AP: 14.7.8.1/24
- SSID: S.O.D‑Link
- Password: S.O.D‑MI7MI8
- SOD Control Panel: http://14.7.8.1:478
- Kismet UI: http://14.7.8.1:2501 (Username: SOD Password: MI7MI8)
- Launch Kismet from SOD Control Panel --external wifi adapter required:
   - Connect phone to SSID “S.O.D‑Link” (password “S.O.D‑MI7MI8”).
   - Visit http://14.7.8.1:478
   - Start “Full” or “Wardrive” mode. Kismet opens on http://14.7.8.1:2501

## Phone GPS via gpsdRelay (F‑Droid)

- Install gpsdRelay (F‑Droid).
- Set UDP Server IPv4: 14.7.8.1, Port: 9999.
- Enable all telemetry settings (NMEA output), allow Location Services, press Play.
- The systemd GPS bridge creates `/dev/phonegps` and runs gpsd on it; Kismet will see GPS from gpsd automatically.

## Menu
- Status
   - Show/Hide (Displays Device Status Information Snapshot)
   - Update (Updates Device Status Information)
- Kismet
   - Start Full (Captures More Data)
   - Start Wardrive (Captures Less Data Good For Capturing Wifi Access Points While Traveling)
   - Stop (Ends The Kismet Capture)
   - Show/Hide View (Shows Snapshot Of Kimset Information)
   - Refresh View (Updates to current information)
- GPS
   - Show/Hide (Displays GPS Information Snapshot)
   - Refresh (Updates GPS Information)
   - Restart (Restarts all GPS Functions)
-WIFI
   - WIFI Client (Will Teardown The S.O.D-Link. S.O.D-Link Will Be Reestablished After Reboot Or Power On)
## Maintenance and Updates

- Easiest: Plug in an external Wi‑Fi adapter, SSH to the device:
  - root/toor or kali/kali
- Or use HDMI + keyboard and run:
  ```
  sudo /root/SOD/Scripts/teardown_sod_link_ap.sh
  ```
  to tear down AP and switch to client Wi‑Fi to update packages.

## Optional LCD UI

- The ST7789 LCD top hat UI provides a status screen, Kismet controls, GPS snapshot, Wi‑Fi toggles, and mini games (Snake/Tetris/Paddle).
- It’s not required for core functionality (AP + Kismet + GPS bridge).
- To access the menu there is a password its joystick up, key1, joystick down, key3, joystick press to unlock it. 
## Troubleshooting

- AP won’t come up:
  - Ensure NetworkManager is installed and enabled.
  - Verify Wi‑Fi interface exists: `lsusb' 'ip a'
- Kismet not seeing the external adapter:
  - Check interfaces with 'ip a' you should see wlan1
- Reset Wi‑Fi to client mode:
  - `sudo /root/SOD/Scripts/teardown_sod_link_ap.sh`

## License

MIT
