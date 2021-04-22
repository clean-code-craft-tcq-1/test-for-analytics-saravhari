# Test for Analytics

Design tests for Analytics functionality on a Battery Monitoring System.

## Analysis-functionality to be tested

This section lists the Analysis for which _tests_ must be written.

Battery Telemetrics are collected and stored on a server.
Data for a month is stored in a [csv file](https://en.wikipedia.org/wiki/Comma-separated_values).

Analysis must be done on this csv file to find the following:
- minimum
- maximum
- count of breaches (how many times did it cross the threshold in a month?)
- record trends (date & time when the reading was continuously increasing for 30 minutes)

A PDF report of the analysis must be stored every week.
Notification must be sent when a new report is available.

## Tasks

### List Dependencies

List the dependencies of the Analysis-functionality.

1. Access to the Server containing the telemetrics in a csv file
1. An external PDF plugin need to be examined and integrate in the server.
1. **Breach dependency:** Input csv data should have the data which crosses the threshold value.
1. **Trend dependency:** Series of csv data which continuously increasing more than 30 minutes.
1. **Notification dependency:** Whether mail based (mail ToAddress, CCAddress, Subject info), alerts (console or controller) based.


### Mark the System Boundary

What is included in the software unit-test? What is not? Fill this table.

| Item                      | Included?     | Reasoning / Assumption
|---------------------------|---------------|---
Battery Data-accuracy       | No            | We do not test the accuracy of data
Computation of maximum      | Yes           | This is part of the software being developed
Off-the-shelf PDF converter | No 			| External stock or provider's plugin which need not to be tested. 
Counting the breaches       | Yes 			| Need to be considered for testing since how many times it crossed the threshold value.
Detecting trends            | Yes			| Need to be considered for testing, since the reading continuously increasing positively more that 30 minutes.
Notification utility        | Yes 			| Test case to check the notification sent with the valid report and acknowledge with the delivery success or not.

### List the Test Cases

Write tests in the form of `<expected output or action>` from `<input>` / when `<event>`

Add to these tests: 

1. Write minimum and maximum to the PDF from a csv containing positive and negative readings.
1. Write "Invalid input" to the PDF when the csv doesn't contain expected data.
1. Write "Empty data" and skip PDF creation when the csv file is empty or null.
1. To check the successful generation of pdf every week.

**Breach Test**
1. Write "Breach exists" when it crossed the threshold value along with the value.
1. write count of breaches when each of the data crosses threshold value in a month.
1. Write "Not a valid data" when the specific data to check breach is null or NaN.

**Trends Test**
1. Check the trend which keep on increased atleast for 30 minutes and return the trend exists or not.
1. Write the trend details (data, time, reading) if it exists more than 30 minutes to PDF.
1. Write "Not a valid data" when the specific data to check the trend is null or NaN.

**Notification Test**
1. Check when any new PDF report available means, trigger the notification.
1. Notification acknowledge back us with successful delivery or not.


### Recognize Fakes and Reality

Consider the tests for each functionality below.
In those tests, identify inputs and outputs.
Enter one part that's real and another part that's faked/mocked.

| Functionality            | Input        | Output                      | Faked/mocked part
|--------------------------|--------------|-----------------------------|---
Read input from server     | csv file     | internal data-structure     | Fake the server store
Validate input             | csv data     | valid / invalid             | None - it's a pure function
Notify report availability | pdf file	  | Send the report through email or alert if new report available			                | Mock the mail/alert trigger
Report inaccessible server | csv file	  | Access denied               | Fake the server store into in-memory
Find minimum and maximum   | csv data 	  | Maximum / Minimum               | None - it's a pure function. Because we are comparing two values and not altering anything in the method keeping it's behavior unchanged.
Detect trend               | date & time, readings | Trend detected with date and time               | None - it's a pure function. Because we are just monitoring the given value by comparing its date/time with next value's date/time to check it keep on increasing atleast for interval of 30 minutes.
Write to PDF               | result reading data set | Analyzed report of "minimum and maximum", "Breach check and count", "Trend detects and details like date and time"               | Mock the pdf plugin write operation if valid data set given 
