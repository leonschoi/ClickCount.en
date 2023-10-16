# Quick Start

The ClickCounter System monitors the work progress by capturing the button click signals from the machine stations and presenting the report of accumulated signals grouped by the station and captured time interval.

ESP32 boards with button switches are used to send the signal via Wi-Fi to the ClickCounter service (a process running in the background).

The ClickCounter service captures and saves the signal, and the ClickTally service processes the saved data to make reports.

The reports are shown on the ClickCounter webpage, and the write time of report files is updated in real-time.

## 1. Template Excel File

Excel files with the cells labeled `station ID`-`time slot ID` (e.g., `st2-103`) are used as a template for the report:

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/812202f5-c357-406c-a9a5-4a429a5adc5b" alt="Template" width="800"/>

These Excel files are converted to HTML report files. The labeled cells in the report files are filled with click counts from the machine `station ID`, recorded during the time interval specified by `time slot ID`.

The template Excel files to be converted to report HTML files are placed in the `\\"Hostname"\ClickTemplate\` directory.

## 2. Click Counter webpage

The ClickCounter webpage can be launched with `http://"Hostname"` on the local network (or `http://localhost` on the installed computer):

<img src="ClickCounter.png" alt="ClickCounter" width="500"/>
<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/812202f5-c357-406c-a9a5-4a429a5adc5b" alt="Template" width="500"/>

The files under the Current section are the files being updated at the time of monitoring. The file names are shown with the last updated time, and listed in the decreasing order of time, so that the user can verify the update progress at a glance.

The webpage is refreshed once a second to show file update changes in real-time. At the bottom of the page the last refresh time is shown.

When the end of the work day is reached, the Current files move to the Archive section with the date added to the end of filenames.

The Archive section files are arranged in the order of decreasing date, and the last updated time is no longer shown.

Each filename can be clicked to view the content. The clicked page will open in a new web browser tab.

## 3. Report webpage

Clicking on a report file name will open a new web browser tab showing the report page. It contains the lastest accumulated click count for the cells that match `station ID`-`time slot ID` label in the template Excel file.

<img src="Report.jpg" alt="Report" width="500"/>

If the page is too big, use Ctrl- to shrink the view.

## 4. Logs

Log files record the status and error information as it executes.

### 4.1. Log

The process information and errors are written to the log file:

<img src="Log.png" alt="Log" width="500"/>

When this webpage is opened or refreshed, the page automatically scrolls to the bottom so that the lastest information is shown.

### 4.2. ESP32 ID to Station ID Mapping

An ESP32 board with a unique serial number is mapped to a machine `station ID`.  The mapping information is obtained from GoogleSheets when the ClickTally service starts. This webpage shows the latest mapping information obtained.

<img src="ESP32.png" alt="ESP32" width="500"/>

### 4.3. Work Days of the Week

Because the ClickTally service run continously in the background, it needs to know when to not process the information.  The working days of the week are specified in the file `C:\YIC\Config\DaysOfWeek.json` (see the ClickTally chapter for more explanation). The information currently used for processing is displayed on the webpage:

<img src="Days.png" alt="Days" width="500"/>

### 4.4. Work Time to Slot ID Mapping

Also due to continuous background process, the work time needs to be specified. Work Time to Slot ID Mapping is used for the working time of the day and also to specify the time slot to accumulate click counts. It is specified in the file `C:\YIC\Config\TimeSlot.json` (see the ClickTally chapter for more explanation). The information currently used for processing is displayed on the webpage:

<img src="TimeSlot.png" alt="TimeSlot" width="500"/>

The slot time interval is measured from the assigned time to the next slot's time. Therefore the final interval time with 'None' as the slot ID is required to mark the end time of the last working slot, i.e., the end of the working day.

### 4.5. Work Calendar

Combining the information from Work Days of the Week and Work Time to Slot ID Mapping, a calendar showing the weekly operation schedule is presented.

<img src="Calendar.png" alt="Calendar" width="500"/>
