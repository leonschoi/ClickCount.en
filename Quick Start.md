# Quick Start

The ClickCounter System monitors workflow progress by capturing button click signals from machine stations and presenting a report of accumulated signals grouped by station and time interval.

It is structured so that users only need to provide Excel files as templates to base the report on. The ClickCounter System will record clicks and update the report, and save the report to the archive at the end of the day. Users can view reports and check progress through the ClickCounter website.

The ClickCounter System runs on configurable business days and hours. The same Excel template files will be used for new reports the next day as long as the files remain in the folder. When an Excel file is deleted, reports related to that Excel file will stop updating.

## 1. Template Excel File

Excel files with cells labeled `station ID`-`time slot ID` (e.g. `st2-103`) are used as a template for the report:

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/812202f5-c357-406c-a9a5-4a429a5adc5b" alt="Template" width="800"/>

These Excel files are converted to HTML report files. The labeled cells in the report files are filled with click count from `station ID`, recorded during the time interval specified by `time slot ID`.

The template Excel files that need to be converted to report HTML files are located in the `\"Hostname"\ClickTemplate\` directory. Files in this directory will be used to generate and update daily reports for as long as it remains there.

## 2. Click Counter webpage

The ClickCounter webpage can be launched using `http://"Hostname"` on the local network (or `http://localhost` on the installed computer):

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/434b95a8-3963-4135-9c62-71753f798df1" alt="ClickCounter webpage" width="500"/>

The files in the `Current` section are files being updated at the time of monitoring. The file names are displayed with the last updated time, and listed in decreasing chronological order so users can quickly verify update progress.

The webpage is automatically refreshed once a second to show file update changes in real time. At the bottom of the page the last refresh time is shown.

At the end of the business day, `Current` files will move to the `Archive` section with the date added to the end of the file name.

The `Archive` section files are sorted in descending date order and the last updated time is no longer displayed.

Each file name can be clicked to view the content. The clicked page will open in a new web browser tab.

## 3. Report webpage

Clicking on the report file name will open a new web browser tab displaying the report page. It contains the latest accumulated click count for cells matching the labels `station ID`-`time slot ID` in the template Excel file.

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/ff65e9bf-ae72-48c6-9b7f-2e23b75500d3" alt="Report" width="800"/>

If the page is too large, use Ctrl- to shrink the view.

## 4. Configuration Files

There are three parameters used to configure system behavior: one parameter for capturing click signals and two parameters for runtime scheduling.

These parameters are rarely changed, so it is not part of the daily workflow.

### 4.1. ESP32 ID to Station ID Mapping

The ESP32 boards with button switches are used to send signals over Wi-Fi to the ClickCounter service (the service is a Windows process running in the background).

The ClickCounter service collects and saves signals and another service in the same machine named ClickTally processes the saved data to generate reports.

The unique serial number of the ESP32 board is mapped to the machine's `station ID`. Mapping information is stored in GoogleSheets and downloaded when the ClickTally service starts. This website displays the latest mapping information obtained.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/eaef97c3-1770-4e02-b3ff-d1eacc0063eb" alt="ESP32" width="500"/>

### 4.2. Work Days of the Week

Because the ClickTally service runs continuously in the background, it needs to know when not to process information. The working days of the week are specified in the file `C:\YIC\Config\DaysOfWeek.json`. The information currently used for processing is displayed on the website:

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/c7cbd128-2fbf-41ce-92b0-5ff087a630e8" alt="Days" width="500"/>

### 4.3. Work Time to Slot ID Mapping

In addition, because the process runs continuously in the background, it is necessary to clearly define the working time. _Work Time to Slot ID Mapping_ is used for the work time of the day and also to specify the time period for accumulating click count. It is stored in the file `C:\YIC\Config\TimeSlot.json`.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/720241fc-b152-4faa-9c68-a8746cebabe2" alt="Time Slot" width="500"/>

The slot time interval is measured from the specified time to the time of the next slot. Therefore, the last period with 'None' as the _slot ID_ is required to mark the end time of the last working slot, i.e. the end of the working day.

### 4.4. Work Calendar

Combining information from _Work Days of the Week_ and _Work Time to Slot ID Mapping_, a calendar showing the weekly activity schedule is presented.

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/e1d7190e-6109-4e89-8648-3a2a4527b539" alt="Calendar" width="500"/>

## 5. Log

Daily process information and errors are written to the log file:

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/ff58c875-d2d6-409d-ae79-95a3fdfe260f" alt="Log" width="500"/>

When this web page is opened or refreshed, it will automatically scroll to the bottom to display the latest information.

