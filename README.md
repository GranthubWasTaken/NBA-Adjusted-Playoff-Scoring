# NBA-data
# IPYNB contains functions to,
 scrape player data from basketball-reference since 1974 for
   * player URL's
   * shooting location data
   * per 100 possession data
   * advanced data
   * play-by-play data (only extending back to 1997)
   
 find multi-year stretches of statistical values for players (and export csv file containing each unique stretch of data for each player) other functions to organize and manipulate data in more digestible or interesting formats.
 
     such as, comparing change in statistical values for each player between regular season and postseason
     e.g. 5 year change in regular season and postseason PTS per 75 (pace-adjusted scoring rate) and TS+ (era/opponent adjusted scoring efficiency) for Kevin Durant from 2015-19.
 Create adjusted playoff efficiency 
  1. Scraping NBA teams' defensive stats since 1974 (TS% allowed [points per possession allowed])
  2. Scraping player URL's since 1974 and using them to scrape their playoff series for their TS%
  3. Dividing a player's TS% by their playoff opponent's regular seasons TS%
  4. Creating total playoff numbers by weighing for MP in each series
 Create adjusted playoff scoring rate
  1. Scraping NBA players' stats since 1974 (league avg. pts per possession for players)
  2. Finding highest league avg. pts per possession (2021)
  3. Creating Scoring Coefficient for each season since 1974. 
  
     Scoring Coefficient = 1 + (1 - (league avg pts per possession / 2021 pts per possession))
     
     ex: 1979 George Gervin   
     1 + (1 - (15.554017 / 16.720268)) = 1.069751
     
  4. Multiply players' PTS per 75 possessions (PP75) by Scoring Coefficient
  
     ex: 1979 George Gervin   
     26.775 * 1.069751 = 28.642583025
