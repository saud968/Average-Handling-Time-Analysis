# Average-Handling-Time-Analysis

![Intro Image](https://github.com/saud968/Average-Handling-Time-Analysis/blob/main/Intro%20Page.png)

## Introduction:
This is a Power BI project on the “Average Handling Time Analysis”. The
project is to analyze and derive insights into the AHT(Average Handling Time), Total Hours, and Difference in Time chat to case closure to make data-driven decisions.

## Problem statement:
1. What is the AHT for the chat and the time taken to close the case after the chat in the current year and months?
2. What is AHT, Difference in Time for the chat to case closure  by day, hours, and months?
3. Which are the top products?
4. Which are the top business groups?

## Skills/concepts demonstrated:
The following Power BI features were incorporated:
-	Bookmarking, 
-	DAX, 
-	Measures, 
-	Page navigation, 
-	Filters,
-	Power Query,
-	Conditional formatting for Background color for Heatmap.

## Features:
1. The heatmap is to identify the AHT, Total AHT for a case, and Difference in Time for the chat to case closure  month-wise, day-wise, and hour-wise.
   ![Heatmap](https://github.com/saud968/Average-Handling-Time-Analysis/blob/main/Heatmap%20-%20AHT.png)

2. Important Dax used to convert text into time format (hh:mm:ss).
   - **1. Chat to Close = 
VAR t = SUBSTITUTE(SUBSTITUTE(SUBSTITUTE([AHT Text], "h", "|"), "m", "|"), "s", "")
VAR d = IF (
        PATHLENGTH(t) = 3,
        DIVIDE(VALUE(pathitem(t, 1)), 24)
            + DIVIDE(VALUE(pathitem(t, 2)), 24 * 60)
            + DIVIDE(VALUE(pathitem(t, 3)), 24 * 60 * 60),
        DIVIDE(VALUE(pathitem(t, 1)), 24 * 60)
            + DIVIDE(VALUE(pathitem(t, 2)), 24 * 60 * 60))
VAR h = ([DATE_CLOSED_UTC_SR] - [DATE_ENTERED_SR_EST] - d) * 24
VAR hr = ROUNDDOWN(h, 0)
VAR m = (h - hr) * 60
VAR mn = ROUNDDOWN(m, 0)
VAR s = ROUNDDOWN((m - mn) * 60, 0) Check for negative values and return an empty string if negative
RETURN
    IF (
        hr < 0 || mn < 0 || s < 0,
        BLANK(),
        TIME(hr, mn, s))**
   - **2. AHT = VAR AHTText = support__cw_ops_manage_case_merge_temp[AHT Text] VAR HoursText = IFERROR(VALUE(LEFT(AHTText, FIND("h", AHTText & "h") - 1)), 0) VAR MinutesStartPos = IFERROR(FIND("h", AHTText) + 2, 1) VAR MinutesEndPos = IFERROR(FIND("m", AHTText & "m", MinutesStartPos), LEN(AHTText)) VAR MinutesText = IFERROR(VALUE(MID(AHTText, MinutesStartPos, MinutesEndPos - MinutesStartPos)), 0) VAR SecondsStartPos = IFERROR(FIND("m", AHTText) + 2, 1) VAR SecondsEndPos = IFERROR(FIND("s", AHTText & "s", SecondsStartPos), LEN(AHTText)) 
VAR SecondsText = IFERROR(VALUE(MID(AHTText, SecondsStartPos, SecondsEndPos - SecondsStartPos)), 0) RETURN FORMAT(TIME(HoursText, MinutesText, SecondsText), "h:mm:ss").**
   - **3. Total Hours = VAR h = ([DATE_CLOSED_UTC_SR] - [DATE_ENTERED_SR_EST]) * 24 VAR hr = ROUNDDOWN(h, 0) VAR m = (h - hr) * 60 VAR mn = ROUNDDOWN(m, 0) VAR s = ROUNDDOWN((m - mn) * 60, 0) RETURN TIME(hr, mn, s)**
   - **4. Conversion into minutes = DITotalMinutesColumn = TIMEVALUE(support__cw_ops_manage_case_merge_temp[Diff4]) * 24 * 60**
   - **5. Conversion into minutes = THFTotalMinutesColumn = TIMEVALUE(support__cw_ops_manage_case_merge_temp[THF2]) * 24 * 60**
   - **6. Conversion into minutes = FormattedAHTInMinutes = VAR AHTText = support__cw_ops_manage_case_merge_temp[AHT Text] VAR HoursText = IFERROR(VALUE(LEFT(AHTText, FIND("h", AHTText & "h") - 1)), 0) VAR MinutesStartPos = IFERROR(FIND("h", AHTText) + 2, 1) VAR MinutesEndPos = IFERROR(FIND("m", AHTText & "m", MinutesStartPos), LEN(AHTText)) VAR MinutesText = IFERROR(VALUE(MID(AHTText, MinutesStartPos, MinutesEndPos - MinutesStartPos)), 0) VAR SecondsStartPos = IFERROR(FIND("m", AHTText) + 2, 1) VAR SecondsEndPos = IFERROR(FIND("s", AHTText & "s", SecondsStartPos), LEN(AHTText)) VAR SecondsText = IFERROR(VALUE(MID(AHTText, SecondsStartPos, SecondsEndPos - SecondsStartPos)), 0) Convert hours and minutes to total minutes VAR TotalMinutes = (HoursText * 60) + MinutesText RETURN TotalMinutes**
   - **7. Average AHT = AVG AHT2 = AVERAGE(support__cw_ops_manage_case_merge_temp[FormattedAHTInMinutes])**

## Visualization:
The report is set on multiple pages as per the requirement:
1.	Overall Page.
2.	First Touch. 
3.	Heatmap.
4.	Raw Data.

## Analysis:

### Note: 
Dax used in features are important to achieve the hh:mm:ss format for AHT from text Data Type and conversion into minutes to apply data in bar and line charts. 
1. Summary Page.
![Summary Page](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Intro%20Page.png)
This page gives us abandon, total chat count, and abandon % month-wise, day-wise, and hour-wise with different visuals like Matrix, Bar and Line Chart, and Small Multiples. 
![Abandon Week by Hours](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Abandon%20by%20Week.png))
![Abandon Month by Hours](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Abandon%20by%20months%20by%20hours.png)
![Abandon Bar and Line Chart](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Abandon%20Overall%20Bar%20and%20Line%20Chart.png)
![Abandon Small Multiples](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Abandon%20Overall%20Bar%20and%20Line%20Chart%20(Small%20Multiples).png)

2. Abandon Heatmap.
![Abandon % Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Abandon%20%25.png)
![Abandon Total Count Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Total%20Chat%20Count.png)
![Abandon Count Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Abandon%20Count.png)
This page gives us abandon, total chat count, and abandon % month-wise, day-wise, and hour-wise in a heatmap version.

3. Pillar Abandon Heatmap.
![New Slicer Button](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/New%20Button%20Slicer.png)
![Abandon % Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Abandon%20%25.png)
![Abandon Total Count Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Total%20Chat%20Count.png)
![Abandon Count Heatmap](https://github.com/saud968/Abandon-Chat-Analysis/blob/main/Heatmap%20Abandon%20Count.png)
This page gives us abandon, total chat count, and abandon % month-wise, day-wise, and hour-wise in a heatmap version. However, it has a new slicer button that allows to separate the business groups which is known as Pillar. 


