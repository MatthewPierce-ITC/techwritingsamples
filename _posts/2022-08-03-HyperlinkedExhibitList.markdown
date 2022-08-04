---
layout: post
title:  "Create a Hyperlinked Exhibit List in Excel"
date:   2022-08-03 10:29:59 -0400
categories: Excel Tutorials
---
**Task:** Create an Excel spreadsheet that has  ID and Description columns, with a row entry for each exhibit.  When the ID is clicked the referenced document should open in its native application.

**Applications Used:** 
 - Microsoft Excel
 - PowerShell
 - TextPad

**Approximate Time to Complete Task:**  10 minutes

Trial exhibits are typically produced in PDF format with lengthy filenames to describe their contents.  While this is convenient for browsing, it limits the user’s ability to search, sort, and take notes.  Excel Spreadsheets and Word Tables are often used for tracking and notes, but this still requires the user to find and open the referenced PDF for review.

A hyperlinked exhibit index makes this task less burdensome and allows the user to focus on the content of the exhibits, rather than their location.  Creating this index is not difficult but requires specialized knowledge of Excel’s features and two other applications to make creation of the spreadsheet quick and easy.

This tutorial assumes that you have Microsoft Excel installed and TextPad by Helios Software Solutions.  TextPad is a third-party program with a fully functional demo available at <a href="https://www.textpad.com" target=_blank>www.textpad.com</a>.  You may use a different text editing program, but it must have column-select capability and Regular Expression search and replace features.

---
<br>

This is the example set of documents that will be processed:

![Screen Grab - Files to Link]({{"/assets/images/01_filesInFolder.png" | relative_url }} )

### Step 1 - Create a Working File

To begin, a text document containing all the files names in the directory is needed.  The easiest way to do this is at the command line.  Hold down the Shift key and right-click on the folder containing the files.  The result is an extended menu that should have the option “Open PowerShell window here” displayed.  

![screen Grab - Open PowerShell]({{"/assets/images/02_openPowerShellAtLocation.png" | relative_url }} )

Selecting this, a PowerShell window will open at the selected location.  Double check that the path displayed in the window is the one that contains the PDF files.

![Screen Grab - Power Shell at Path]({{"/assets/images/03_powerShellAtPath.png" | relative_url }} )

Type the following command at the PowerShell prompt and press Enter:
```
cmd /r dir *.pdf /o/b > allFiles.txt
```
![Animation - PowerShell enter command]({{"/assets/images/04_enterPowerShellCommand.gif" | relative_url }} )

This tells PowerShell to run the directory list command (dir) to only display PDF files (*.pdf), in order (/o), with just the filename (/b), and redirect the output to a new text file (> allFiles.txt) rather than the screen.

> This is command itself is powerful and has several options which can be used in a variety of situations.  There are many third-party utilities that allow users to print the contents of a folder.  As we see here, it can be done with ease using PowerShell – and it is free.  Use the command **cmd /r dir /?** for more information on the DIR command 

You may close the PowerShell window at this time by typing **exit** and hitting the Enter key, or simply click the X at the top of the window.

Opening the resulting allfiles.txt file, you should see this:

![Screen Grab - Directory Output]({{"/assets/images/05_directoryOutput.png" | relative_url }} )

As mentioned earlier, TextPad will be used for this tutorial, which has many more features necessary for the following steps.

![Screen Grab - TextPad]({{"/assets/images/06_textPad.png" | relative_url }} )

### Step 2 - Build the Excel Spreadsheet

Save the .txt file as .CSV (Comma Separated Values) and change the default Encoding to DOS.  This has an impact on how other programs interpret the hidden new line and carriage return characters.  Additionally, We will be returning to the unmodified allFiles.txt file later and need its contents to remain as they are now.

![Screen Grab - Save as CSV]({{"/assets/images/07_saveAsCSV.png" | relative_url }} )

Now modifications need to be made to the contents of the file.  In TextPad press the F8 key to bring up the Search and Replace window.

Replace the “ – “ [space hyphen space] in each line with ”,” [quote comma quote] to separate the ID from the Description in the filenames.

![Screen Grab - Replace Hyphen with Comma]({{"/assets/images/08_replaceHyphenWithComma.png" | relative_url }} )

Using the Regular Expression feature, add a quotation mark to the beginning of each line.  The carat ^ (Shift + 6) character represents the beginning of the line in Regular Expressions.  Be sure to select the checkbox for Regular Expressions before you click Replace All.

![Screen Grab - Quote at Line Start ]({{"/assets/images/09_quoteAtLineStart.png" | relative_url }} )

Remove the .pdf extension from the end of each line, it is not needed in the Description field of the finished Excel Spreadsheet.  Uncheck the Regular Expression option and replace .PDF with a single quotation mark.

![Screen Grab - Strip PDF Extension]({{"/assets/images/10_stripPDFExtention.png" | relative_url }} )

Because we saved the working file with the .csv extension, it should automatically open with Excel if you double-click on the file.  If your computer does not default to Excel, simply open Excel, then open the allFiles.csv file.  You should see the ID in column A and the Description in column B.

![Screen Grab - Open in Excel]({{"/assets/images/11_openInExcel.png" | relative_url }} )

### Step 3 - Create the Hyperlink Data

For the hyperlinking to work and open the appropriate PDF file when the ID in column A is clicked, the cell needs to have the proper contents.  Because we have the files in a folder named **exhibits**, the link should look like this:
```
=HYPERLINK(“exhibits\Microsoft’s Actual and Projected Share of PC Operating System Marked.pdf”,”US Ex. 0001”)
```
One could manually type in this data for each line in the Excel file, but it would take many hours to do so.

Going back to the original allFiles.txt file, a series of operations must be preformed so that each line is modified to look like this:

![Screen Grab - Hyperlink Text Example]({{"/assets/images/12_hyperlinkExample.png" | relative_url }} )

We will be using the Regular Expression (RegEx) search and replace feature with the ^ [carat] character to append text to the beginning of each line.  Because RegEx has several characters that are reserved for special tasks, we must “escape” a couple of the characters so that they are treated as literals.  Namely the opening parenthesis “(” and the backslash “\”.

In order to have the text: 
```
=HYPERLINK(“exhibits\
```
The RegEx command requires:  
```
=HYPERLINK\(“exhibits\\
```

By placing the \ character before these reserved characters, we have informed RegEx that we want those characters to be present in the replace operation result.

![Animation - Add Hyperlink Prefix]({{"/assets/images/13_addHyperlinkPrefix.gif" | relative_url }} )

This first entry enclosed in quotes tells Excel where the documents are located relative to the Excel file.  The second informs Excel what text to display in the cell that will be the hyperlink text.  To do this, append “,” [quote-comma-quote] to the end of each line by searching for .PDF and replacing it with .pdf”,”.  Be sure to turn off the Regular Expression checkbox before you click Replace All

> If you are linking non-PDF files, such as .JPG and .MP4, this will require you to do multiple operations.  Rather than searching for .PDF, you can append the needed characters to the end of each line via RegEx.  Search for the RegEx end of line character \n and replace with “,”\n.

![Screen Grab - Add Hyperlink Suffix]({{"/assets/images/14_addHyperlinkSuffix_A.png" | relative_url }} )

Now your file should look something link this:

![Screen Grab - Add Hyperlink Suffix Result]({{"/assets/images/15_addHyperlinkSuffix_B.png" | relative_url }} )

To get the IDs at the end of each line, use the Block Select option of TextPad to select several columns of text.  Right-click and select “Block Select Mode” to turn on this feature.

![Screen Grab - Turn On Block Select Mode]({{"/assets/images/16_turnOnBlockSelectMode.png" | relative_url }} )

Now place the cursor at the start of the ID on the first line, scroll down to the bottom of the file, hold down the Shift key and click the last line at the end of the ID.

![Animaiton - Block Select IDs]({{"/assets/images/17_selectIDColumn.gif" | relative_url }} )

With the selection active, use Control + C, or right-click > Copy to copy the text.  Now scroll to the right in the text file and find a point where no text will be impacted when the copied selection is pasted.  Use the horizontal and vertical scroll bars to identify the best place, click on the first line and paste the IDs.  Then use the scroll bars again to make sure that the inserted IDs do not intersect any of the Descriptions.

![Animation - Paste IDs]({{"/assets/images/18_pasteIDColumnAtEnd.gif" | relative_url }} )

The next step is to remove the whitespace between the “,” [quote-comma-quote] after the Description and the pasted IDs.  Again, Regular Expressions make this easy.  Bring up the Search and Replace window with the F8 key again and put “\t in the “Find what” field.  This tells TextPad to search for all quotation marks followed by a tab character.  The replacement value is simply a quotation mark, which will eliminate the unwanted tabs with repeated clicks of the Replace All button.

![Animaiton - Remove Tab Characters]({{"/assets/images/19_removeTabs.gif" | relative_url }} )

> It is possible that your placement of the IDs during the Paste operations resulted in a few spaces being present after the last tab character.  If your ID is not flush against the quotation mark after all the tabs are replaced, search for quotation-space (“ ) and replace with a single quotation mark as we did with the tab character.

Now the closing text for the hyperlink needs to be added.  Again, with Regular Expressions this is a simple task, but special characters need to be accounted for.  Search for the end of line character \n and replace with “\)\n [quote-close parenthesis-end of line].

![Animation - Add Hyperlink End Characters]({{"/assets/images/20_addHyperlinkSuffix_C.gif" | relative_url }} )

## Step 4 - Import Hyperlinks into Excel

At this point save the allFiles.txt file and return to the Excel sheet.  Insert a new column to the left of the existing ID.

Copy the entire contents of the allFiles.txt file, now formatted to be hyperlinks for Excel, and paste them into column A of the worksheet.

![Screen Grab - Turn OFf Block Select]({{"/assets/images/assets/22_turnOffBlockSelectAndCopy.png" | relative_url }} )

The result should have the contents of column A displaying only the ID, but when an individual cell is selected the formula bar should display the hyperlink information.

![Screen Grab - Past IDs into Excel]({{"/assets/images/23_pasteIntoColumnA.png" | relative_url }} )

## Step 5 - Save and Format Excel Sheet

Now save the .csv file as an Excel Workbook.  This will allow you to make further modifications to the formatting of the spreadsheet.

![Screen Grab - Save as Excel]({{"/assets/images/24_saveAsExcel.png" | relative_url }} )

Changing the ID cells to have a blue font and and adding an underline will alert the users that these are hyperlinks.  Clicking on them individually at this point will open the respective files.

![Screen Grab - Excel Formatting]({{"/assets/images/25_formatTextAndColumns.png" | relative_url }} )

It is important that the Excel file reside in the same folder as the “exhibits” folder.  The Excel file and exhibit folder can be moved to any other location, even an external drive, and still function.  The paths are relative to one another, and not impacted if placed in further sub-directories.

![Screen Grab - Relative file location ]({{"/assets/images/26_relativeLocations.png" | relative_url }} )