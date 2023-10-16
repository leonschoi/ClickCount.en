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

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/434b95a8-3963-4135-9c62-71753f798df1" alt="ClickCounter webpage" width="500"/>

The files under the Current section are the files being updated at the time of monitoring. The file names are shown with the last updated time, and listed in the decreasing order of time, so that the user can verify the update progress at a glance.

The webpage is refreshed once a second to show file update changes in real-time. At the bottom of the page the last refresh time is shown.

When the end of the work day is reached, the Current files move to the Archive section with the date added to the end of filenames.

The Archive section files are arranged in the order of decreasing date, and the last updated time is no longer shown.

Each filename can be clicked to view the content. The clicked page will open in a new web browser tab.

## 3. Report webpage

Clicking on a report file name will open a new web browser tab showing the report page. It contains the lastest accumulated click count for the cells that match `station ID`-`time slot ID` label in the template Excel file.

<img src="https://github.com/leonschoi/ClickCounter.en/assets/29897968/b32f9563-29af-4dae-b19e-d833717877d5" alt="Report" width="800"/>


If the page is too big, use Ctrl- to shrink the view.

## 4. Logs

The process information and errors are written to the log file:

<img src="https://github.com/leonschoi/ClickCount.en/assets/29897968/ff58c875-d2d6-409d-ae79-95a3fdfe260f" alt="Log" width="500"/>

When this webpage is opened or refreshed, the page automatically scrolls to the bottom so that the lastest information is shown.
