# NBA-data
# IPYNB contains functions to,
 scrape player data from basketball-reference since 1974 for
   * player URL's
   * shooting location data
   * per 100 possession data
   * advanced data
   * play-by-play data (only extending back to 1997)
   
 find 2, 3, 5, 8 and 10 year stretches of statistical values for players (and export csv file containing each unique stretch of data for each player) other functions to organize and manipulate data in more digestible or interesting formats.
 
     such as, comparing change in statistical values for each player between regular season and postseason
     e.g. 5 year change in regular season and postseason PTS per 75 (pace-adjusted scoring rate) and TS+ (league adjusted scoring efficiency) for Kevin Durant from 2015-19.
 Create adjusted playoff efficiency 
  1. scraping NBA team's defensive stats since 1974 (TS% allowed [points per possession allowed])
  2. grabbing player URL's since 1974 and scraping their playoff series for their TS%
  3. Dividing a player's TS% by their playoff opponent's regular seasons TS%
  4. Creating total playoff numbers by weighing for MP in each series
