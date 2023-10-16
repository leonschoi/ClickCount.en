# Quick Setup

## Windows

Once all installation files are copied into destination directories and IIS is configured (see the Installation sections of the 4 component chapters), ClickCounter system can be brought up or down quickly as follows:

### Web Server

- Go to the `Internet Information Services (IIS) Manager` app (type `IIS` in the taskbar search box)\
  <img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/3474b66e-9c66-4c89-9a90-79b40c8d96d8" alt="IIS Start" width="500"/>
- On the left pane of `Internet Information Services (IIS) Manager`, expand `"Hostname" (Hostname\User)` > `Sites`\
  <img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/ca683859-79d6-42a2-8e5e-3f629a234a01" alt="IIS Manager" width="700"/>
- Click on the `ClickCounter` site.  The `Actions` pane opens on the right side.
- In the `Manage Website` section, click on `Start` if the website is on `Stop`.

### ClickCounter and ClickTally Services

- Go to the `Administrator: Command Prompt` window (type `cmd` in the taskbar search box, right click on the `Command Prompt App`, select `Run as administrator`)\
  <img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/9cef7aa1-f8c9-411e-95ce-c8e10740c7d0" alt="CMD Start" width="500"/>
- In the prompt window, run `sc.exe start "ClickCounter"`\
  <img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/650e00c7-7ae8-4f27-9384-cd05698ac8dd" alt="CMD Prompt" width="800"/>
  - Two cases are shown: if the service `ClickCounter` was not running, and if it was already running.
  - Also shown are the two cases for `sc.exe stop "ClickCounter"` to stop the service.
- Run `ClickTally` by executing `sc.exe start "ClickTally"`. The response will be the same as `ClickCounter`.

### Load Template Excel Files

In Windows Explorer of any computer in the local network shared by the computer `"Hostname"`, type `\\"Hostname"` to see the content of accesible directories\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/b341e510-56f3-4eec-9394-84b7aa3edf35" alt="Hostname" width="700"/>

Click on `ClickTemplate` or keep typing to complete `\\"Hostname"\ClickTemplate` then `Enter`.\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/e8801590-321c-4670-9fac-d07d049dcb22" alt="ClickTemplate" width="700"/>

Copy the template Excel files into this directory to be processed by `ClickTally`.  A file can be added or removed any time. When a file is added, `ClickTally` will convert it to a report HTML file and include it in the process.  When a file is removed, the corresponding report HTML file will remain but it will not be included in the further process.  

## ESP32

Connect the `"Hostname"` computer to an ESP32 board with a micro USB cable. In Windows Explorer, go to `C:\YIC\ESP32_click_counter\` directory:\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/48faf241-d25c-4b95-847e-3e932763358e" alt="ESP32_click_counter" width="700"/>

Double click on the `ESP32_click_counter.ino` file to bring up Arduino IDE with the files in the directory.  Select `config.cpp`\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/1c8deff4-7414-40d6-8f7e-453f526cd8a1" alt="config.cpp" width="800"/>

`HostInfo` type is used to declare a variable that contains the host computer name and port number. Using `DESKTOP-0AJUHED` as the host name, set:

```C++
//
// Host computer name and port number
//
HostInfo host1("DESKTOP-0AJUHED", PORT_NUMBER);
```

`LoginInfo` array contains a list of Wi-Fi loginID/password and host computer in the network. Set the `ssid` and `password` that the host computer `DESKTOP-0AJUHED` is connected to:

```C++
//
// WiFi login/pw and ClickCounter host to connect on the network
//
LoginInfo loginInfoStore[MAX_LOGIN_INFO_SIZE] {
  // ssid     password               host
  {"YIC Office", "enter password here", &host1}
};
```

Now click on the `config.h` tab:\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/1fa9bfa3-3547-4731-8b41-b36b9cb26793" alt="config.h" width="800"/>

`PORT_NUMBER` is 8201 for ClickCounter.\
`MAX_LOGIN_INFO_SIZE` is used to specify the Wi-Fi loginID/password list entry count. In this case, `=1` since there is only one entry:

```C++
//
// Wi-Fi and host PC info variables
//
const int PORT_NUMBER = 8201; 
const int MAX_LOGIN_INFO_SIZE = 1;
```

The following items must be set before running the program. They tend to be unselected when the environment changes, hence need to be checked often.

Board selection:\
Tools > Board > esp32 > ESP32 Dev Module\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/e4bc7f77-f921-44e2-bbd0-b8a4c37760b0" alt="ESP32 Board" width="800"/>  

COM port selection:\
Tools > Port > COM#\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/38a0f185-f2ea-4a15-bb15-68f4e624288f" alt="ESP32 Port" width="800"/>  

Serial Monitor window toggle:\
Tools > Serial Monitor (Ctrl-Shift-M), or click on the right magnifying glass icon.

On the right side of the Serial Monitor window:\
Set COM baud rate to 115200 baud\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/2269de6d-1d80-426d-ba8b-63ef94c5a4ff" alt="Serial Monitor" width="800"/>  

Compile the program in one of two ways:

1. On the menu:\
   Sketch > Verify/Compile (Ctrl-R)
2. On the top toolbar, click on the first button showing the check sign.

Compilation messages are shown on the `Output` window.

If it compiles, upload the program in one of two ways:

1. On the menu:\
   Sketch > Upload (Ctrl-U)
2. On the top toolbar, click on the second button showing the right arrow.

Now the ESP32 board is running and the interaction messages will show on the Serial Monitor window.\
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/0bb327d0-9aa4-4ed3-98f6-883c3b9ac78f" alt="ESP32 Run" width="800"/>  
