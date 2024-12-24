# Export File Names from Google Drive Folder

## Overview

Using the GUI to manage large Google Drive folders can be slow, especially with hundreds or thousands of files. Alternatively, downloading the files to extract metadata (since the GUI doesn't support metadata extraction) would be time-consuming, storage-intensive and bandwidth-heavy. This guide aims to solve this by helping you export the names of files from a Google Drive folder into a downloadable Google Sheet, without needing to download the actual files.

## Prerequisites

Before you begin, ensure that you have the following:

- A Google account.
- A Google Drive folder containing the files you want to extract names from.

## Steps

### 1. Create a Google Sheet
1. Open [Google Sheets](https://sheets.google.com) and create a new spreadsheet.

### 2. Use Google Apps Script to List File Names
1. Open the Google Sheet you just created.
2. Go to the **Extensions** menu and select **Apps Script**.
3. Delete any default code in the script editor.
4. Choose one of the two options listed here and paste the code into the script editor.
   - Option 1: Export file names only.
     ```javascript
      function listFileNames() {
        var folderId = 'YOUR_FOLDER_ID';  // Replace with your Google Drive folder ID
        var folder = DriveApp.getFolderById(folderId);
        var files = folder.getFiles();
        var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
      
        // Add headers to the first row
        sheet.appendRow(['File Name']);
      
        // Loop through all files and list their names in the sheet
        while (files.hasNext()) {
          var file = files.next();
          var fileName = file.getName();
          sheet.appendRow([fileName]);  // Add each file name to a new row
        }
      }
      ``` 
   - Option 2: Export file names along with their starred status.
     ```javascript
     function listFileNamesAndStarred() {
       var folderId = 'YOUR_FOLDER_ID';  // Replace with your Google Drive folder ID
       var folder = DriveApp.getFolderById(folderId);
       var files = folder.getFiles();
       var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
     
       // Add headers to the first row
       sheet.appendRow(['File Name', 'Starred']);
     
       // Loop through all files and list their names and starred status in the sheet
       while (files.hasNext()) {
         var file = files.next();
         var fileName = file.getName();
         var isStarred = file.isStarred() ? 1 : 0;  // Check if the file is starred
         sheet.appendRow([fileName, isStarred]);  // Add file name and starred status to a new row
       }
     }
     ```

### 3. Get the Folder ID
1. Go to your Google Drive and navigate to the folder that contains the files.
2. The folder ID is the long string of characters after `/folders/` in the URL.
   - Example: If your folder URL is `https://drive.google.com/drive/folders/abc123xyz?resourcekey=123456789`, the folder ID is `abc123xyz`.
3. Replace `'YOUR_FOLDER_ID'` in the script with your folder's actual ID.

### 4. Run the Script
1. Save the script by clicking the **Save** icon (floppy disk).
2. Click the **Run** button (triangle icon) to execute the script.
3. If prompted, grant the necessary permissions for the script to access your Google Drive.

### 5. Check the Google Sheet
- Once the script finishes running, you'll see the list of file names in the first column of the active sheet.

### 6. Export the File Names
You can now download the list of file names in a format of your choice (CSV, Excel, etc.):
1. Go to **File > Download**.
2. Choose your preferred format (e.g., `.csv` or `.xlsx`).

### Further Work
- You can easily manipulate this list file names using other tools (e.g. databases, spreadsheets).

### Troubleshooting
- If you're processing a very large number of files, make sure you're within Google Apps Script's API limits. You might need to batch the files or add error handling for larger folders.
- Ensure that the script has sufficient permissions to access files within your Google Drive.

### References
- [Google Apps Script documentation](https://developers.google.com/apps-script/guides/drive)
- [Google Sheets API documentation](https://developers.google.com/sheets/api)
