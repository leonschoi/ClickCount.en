# Quick Start

The ClickCounter System monitors the work progress by capturing the button click signals from the machine stations and presenting the report of accumulated signals grouped by the station and time interval.

It is structured so that a user simply needs to supply Excel files as templates to base the reports on. The ClickCounter System will record the clicks and update the reports, and save the reports to archive at the end of the day. The user can view the reports and check the progress through ClickCounter website.

The ClickCounter System runs according to configurable work hours and days. The same template Excel files will be used for the new report next day as long as the files remain in the directory.  When the Excel files are removed it will stop updating the reports associated with the Excel file.

## 1. Template Excel File

Excel files with the cells labeled `station ID`-`time slot ID` (e.g., `st2-103`) are used as a template for the report:

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/812202f5-c357-406c-a9a5-4a429a5adc5b" alt="Template" width="800"/>

These Excel files are converted to HTML report files. The labeled cells in the report files are filled with click counts from the machine `station ID`, recorded during the time interval specified by `time slot ID`.

The template Excel files to be converted to report HTML files are placed in the `\\"Hostname"\ClickTemplate\` directory. The files in this directory will be used to generate and update daily reports as long as it remains there.

## 2. Click Counter webpage

The ClickCounter webpage can be launched with `http://"Hostname"` on the local network (or `http://localhost` on the installed computer):

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/434b95a8-3963-4135-9c62-71753f798df1" alt="ClickCounter webpage" width="500"/>

The files under the Current section are the files being updated at the time of monitoring. The file names are shown with the last updated time, and listed in the decreasing order of time, so that the user can verify the update progress at a glance.

The webpage is refreshed once a second to show file update changes in real-time. At the bottom of the page the last refresh time is shown.

When the end of the work day is reached, the `Current` files move to the `Archive` section with the date added to the end of filenames.

The `Archive` section files are arranged in the order of decreasing date, and the last updated time is no longer shown.

Each filename can be clicked to view the content. The clicked page will open in a new web browser tab.

## 3. Report webpage

Clicking on a report file name will open a new web browser tab showing the report page. It contains the lastest accumulated click count for the cells that match `station ID`-`time slot ID` label in the template Excel file.

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/ff65e9bf-ae72-48c6-9b7f-2e23b75500d3" alt="Report" width="800"/>

If the page is too big, use Ctrl- to shrink the view.

## 4. Configuration Files

There are three parameters used to configure the running of the system: one for capturing the click signal and two for scheduling the runtime.

These parameters will be changed rarely, hence it is not a part of everyday workflow.

### 4.1. ESP32 ID to Station ID Mapping

ESP32 boards with button switches are used to send the signal via Wi-Fi to the ClickCounter service (service is a Windows process running in the background).

The ClickCounter service captures and saves the signal, and another service in the same machine called ClickTally processes the saved data to make reports.

ESP32 board's unique serial number is mapped to a machine `station ID`.  The mapping information is stored in GoogleSheets and downloaded when the ClickTally service starts. This webpage shows the latest mapping information obtained.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/eaef97c3-1770-4e02-b3ff-d1eacc0063eb" alt="ESP32" width="500"/>

### 4.2. Work Days of the Week

Because the ClickTally service runs continously in the background, it needs to know when to not process the information.  The working days of the week are specified in the file `C:\YIC\Config\DaysOfWeek.json`. The information currently used for processing is displayed on the webpage:

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/c7cbd128-2fbf-41ce-92b0-5ff087a630e8" alt="Days" width="500"/>

### 4.3. Work Time to Slot ID Mapping

Also due to continuous background process, the work time needs to be specified. Work Time to Slot ID Mapping is used for the working time of the day and also to specify the time slot to accumulate click counts. It is specified in the file `C:\YIC\Config\TimeSlot.json`.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/720241fc-b152-4faa-9c68-a8746cebabe2" alt="Time Slot" width="500"/>

The slot time interval is measured from the assigned time to the next slot's time. Therefore the final interval time with 'None' as the slot ID is required to mark the end time of the last working slot, i.e., the end of the working day.

### 4.5. Work Calendar

Combining the information from _Work Days of the Week_ and _Work Time to Slot ID Mapping_, a calendar showing the weekly operation schedule is presented.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/e1d7190e-6109-4e89-8648-3a2a4527b539" alt="Calendar" width="500"/>

## 5. Log

The daily process information and errors are written to the log file:

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/ff58c875-d2d6-409d-ae79-95a3fdfe260f" alt="Log" width="500"/>

When this webpage is opened or refreshed, the page automatically scrolls to the bottom so that the lastest information is shown.
