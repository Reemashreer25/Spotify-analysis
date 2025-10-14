# SPOTIFY ANALYSIS
Overview 

This project demonstrates an end-to-end data analysis of Spotify music streaming data using Microsoft Power BI. The main goal is to transform raw data into interactive visual insights that reveal listener behaviour, track and album performance, and artist popularity. 

The report leverages dynamic visuals, slicers, and images to allow users to explore . 

* Data Transformations 

* Imported Data 

* Spotify Listening History (2013–2024) 

Key columns include: 

* track_name, album_name, artist_name,Year. 

* track played date, track played time. 

* No of albums, No of artists , No of Tracks. 

 

Data Cleaning & Transformation 

Handled missing or inconsistent values. 

Created a Date Table and derived fields like Year, Day Number, Weekday_Weekend, Day Name. 

Calculated key metrics using DAX measures for top tracks, top albums, top artists, and listening trends. 

 

Data Model 

The data model connects tracks, albums, artists, and listening history for accurate aggregation: 

* Tracks Table: Track-level information including audio features. 

* Albums Table: Album-level metrics such as  total streams. 

* Artists Table: Summarizes total streams, number of tracks, and popularity. 

* Listening History Table: User listening events with timestamps. 

 

## Dashboard Pages and Analytics 

## 1.Tracks, Albums, and Artists Page 

Purpose: Explore individual tracks, albums, and artists to identify top performers. 

Visualization: dynamic images for tracks, album covers, and artist photos. Users can filter by artist, album, or track using slicers and KPIs is used 

###DAX QUERY 

Albums: 

Year = YEAR('Date Table'[Date]) 

Date Table = CALENDAR(MIN('spotify_history (1)'[Track Played Date]),MAX('spotify_history (1)'[Track Played Date])) 

Day Number = WEEKNUM('Date Table'[Date]) 

Weekday_Weekend = IF(WEEKDAY('Date Table'[Date], 2)<=5, "Weekday","Weekend") 

Day Name = FORMAT('Date Table'[Date], "DDD") 

Max Year = MAX('Date Table'[Year]) 

LatestYearAlbums =  

VAR _LatestYear = MAX('Date Table'[Year]) 

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[album_name]), 'Date Table'[Year] = _LatestYear) 

MinMax Albums Line Chart =  

VAR _MaxValue =  

MAXX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[album_name]))) 

VAR _MinValue = 

 MINX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[album_name]))) 

VAR _CurrentValue =  

DISTINCTCOUNT('spotify_history (1)'[album_name]) 

RETURN 

  IF(_CurrentValue = _MaxValue || _CurrentValue = _MinValue, _CurrentValue, BLANK()) 

No of Albums = DISTINCTCOUNT('spotify_history (1)'[album_name]) 

PreviousYearAlbums =  

VAR _LatestYear = MAX('Date Table'[Year]) 

VAR _PreviousYear =  _LatestYear -1  

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[album_name]),'Date Table'[Year]=_PreviousYear) 

PY and YOY Albums KPI =  VAR _latest = [LatestYearAlbums] 
VAR _previous = [PreviousYearAlbums] 

VAR _YOY = IF(NOT(ISBLANK(_previous)), DIVIDE(_latest_previous, _previous,0), BLANK()) 

RETURN 

IF(NOT(ISBLANK(_previous)), 

"vs PY: " & FORMAT(_previous, "#,##0") &  

 " (" & FORMAT(_YOY, "0.00%") & ")", 

 "No Data") 

 

 

Artists  

LatestYearArtists =  

VAR _LatestYear = MAX('Date Table'[Year]) 

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[artist_name]), 'Date Table'[Year] = _LatestYear) 

MinMax Artists Line Chart =  

VAR _MaxValue = MAXX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[artist_name]))) 

VAR _MinValue = MINX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[artist_name]))) 

VAR _CurrentValue = DISTINCTCOUNT('spotify_history (1)'[artist_name]) 

RETURN 

  IF(_CurrentValue = _MaxValue || _CurrentValue = _MinValue, _CurrentValue, BLANK()) 

No of Artists = DISTINCTCOUNT('spotify_history (1)'[artist_name]) 

PreviousYearArtists =  

VAR _LatestYear = MAX('Date Table'[Year]) 

VAR _PreviousYear =  _LatestYear -1  

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[artist_name]),'Date Table'[Year]=_PreviousYear) 

PY and YOY Artists KPI =  

VAR _latest = [LatestYearArtists] 

VAR _previous = [PreviousYearArtists] 

VAR _YOY = IF(NOT(ISBLANK(_previous)), DIVIDE(_latest - _previous, _previous,0), 

BLANK()) 

RETURN 

IF(NOT(ISBLANK(_previous)), 

"vs PY: " & FORMAT(_previous, "#,##0") &  

 " (" & FORMAT(_YOY, "0.00%") & ")", 

 "No Data") 

 

Tracks 

LatestYearTracks =  

VAR _LatestYear = MAX('Date Table'[Year]) 

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[track_name]), 'Date Table'[Year] = _LatestYear) 

MinMax Tracks Line Chart =  

VAR _MaxValue = MAXX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[track_name]))) 

VAR _MinValue = MINX(ALLSELECTED('Date Table'[Year]),CALCULATE(DISTINCTCOUNT('spotify_history (1)'[track_name]))) 

VAR _CurrentValue = DISTINCTCOUNT('spotify_history (1)'[track_name]) 

RETURN 

  IF(_CurrentValue = _MaxValue || _CurrentValue = _MinValue, _CurrentValue, BLANK()) 

No of Tracks = DISTINCTCOUNT('spotify_history (1)'[track_name]) 

PreviousYearTracks =  

VAR _LatestYear = MAX('Date Table'[Year]) 

VAR _PreviousYear =  _LatestYear -1  

RETURN 

CALCULATE(DISTINCTCOUNT('spotify_history (1)'[track_name]),'Date Table'[Year]=_PreviousYear) 

PY and YOY Tracks KPI =  

VAR _latest = [LatestYearTracks] 

VAR _previous = [PreviousYearTracks] 

VAR _YOY = IF(NOT(ISBLANK(_previous)), DIVIDE(_latest - _previous, _previous,0), 

BLANK()) 

RETURN 

IF(NOT(ISBLANK(_previous)), 

 "vs PY: " & FORMAT(_previous, "#,##0") &  

 " (" & FORMAT(_YOY, "0.00%") & ")", 

  "No Data") 


 <img width="700" height="700" alt="Screenshot 2025-10-10 185647" src="https://github.com/user-attachments/assets/dd3968f0-2433-46d2-942f-de89628cc96a" />





## 2.Listening Pattern 

Purpose: This section focuses on understanding user listening behaviour across different time periods.  

Visualization: The Listening Pattern page uses a combination of heat maps, line charts, and quadrant visualizations to analyse. 

 

### DAX QUERY 

Day Number in week = WEEKDAY('Date Table'[Date],2) 

Hour = HOUR('spotify_history (1)'[Track Played Time]) 

Average Listening Time (min) = AVERAGE('spotify_history (1)'[ms_played])/60000 

CF Quadrant =  

VAR AvgTime = [Average Listening Time (min)] <= 'Listening Time (min)'[Listening Time (min) Value] 

VAR TrackFreq = [Track frequency] >= 'Track frequency (Parameter)'[Track frequency (Parameter) Value] 

VAR Result =  

  SWITCH( 

   TRUE(), 

   AvgTime && TrackFreq ,1,  --LowTime & high Freq 

   NOT AvgTime && TrackFreq, 2, --High Time & hig Freq 

   NOT AvgTime && NOT TrackFreq, 3,  --High Time & Low Freq 

   AvgTime && NOT TrackFreq, 4,  --low time and Low Freq 
   
   BLANK() 

   )  

   RETURN Result 

Track Frequency = COUNTROWS('spotify_history (1)') 

Track Played Date = DATE(YEAR('spotify_history (1)'[ts]),MONTH('spotify_history (1)'[ts]),DAY('spotify_history (1)'[ts])) 

Track Played Time = FORMAT('spotify_history (1)'[ts],"HH:MM;SS") 

Track frequency (Parameter) = GENERATESERIES(0, 200, 1) 

Track frequency (Parameter) Value = SELECTEDVALUE('Track frequency (Parameter)'[Track frequency (Parameter)]) 


<img width="700" height="700" alt="Screenshot 2025-10-10 185711" src="https://github.com/user-attachments/assets/5d7f217d-8667-4649-a2a0-8efcce2992c3" />


## 3. Details page  

Purpose: To analyse detailed data records rather than summary KPIs. To allow drill-down, drill-up analysis for understanding specific listening sessions. 

Visualization: The Details page primarily uses a table visual supported by interactive slicers. 

<img width="700" height="700" alt="Screenshot 2025-10-10 185738" src="https://github.com/user-attachments/assets/900b913d-49e8-46f6-b8ca-1f55be67904a" />

<img width="700" height="700" alt="Screenshot 2025-10-10 190819" src="https://github.com/user-attachments/assets/86b2883d-05fd-4328-b745-15b9a5e387b5" />
