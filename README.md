# NBA-data
# IPYNB contains functions to,
 scrape player data from basketball-reference since 1952 (excluding 1954*) for
   * player URL's
   * shooting location data (only extending back to 1997)
   * per 100 possession data
   * advanced data
   * play-by-play data (only extending back to 1997)
   
 find multi-year stretches of statistical values for players (and export csv file containing each unique stretch of data for each player) other functions to organize and manipulate data in more digestible or interesting formats.
 
     such as, comparing change in statistical values for each player between regular season and postseason
     e.g. 5 year change in regular season and postseason PTS per 75 (pace-adjusted scoring rate) and TS+ (era/opponent adjusted scoring efficiency) for Kevin Durant from 2015-19.
     
     
     
 Create adjusted playoff efficiency (TS+)
 
  Reasoning: Players' raw scoring stats cannot be accurately compared across various periods of NBA history nor various qualities of opponent defense.
  

    
        2004 Kobe Bryant vs San Antonio Spurs (24.48 Pts per 75 possessions (PP75); .5304 TS%)
        
        2022 Jayson Tatum vs Brooklyn Nets (27.15 Pts per 75 possessions (PP75); .6146 TS%)
      
  Jayson Tatum has higher scoring rate and efficiency. Both conventional measures of scoring effectiveness. Is this evidence Tatum had a better scoring series?
  
  Basic theory on adjusting for Opponent Quality/Era: 
  
  #Adjusted Efficiency#
  
  1. Scraping NBA teams' defensive stats since 1974 (TS% allowed [points per possession allowed]).
  2. Scraping player URL's since 1952 (> 1000 MP) and using their basketball-reference player pages' organized playoff series data table to total playoff scoring.
   * Dividing a player's TS% by their playoff opponent's regular seasons TS%; series-by-series


      
          2004 Kobe Bryant vs. Spurs
          (Kobe Bryant TS% / Spurs TS% allowed in regular season) * 100
          (0.5304 / 0.4798) * 100 = 110.54 TS+
          
          2022 Jayson Tatum vs. Nets
          (Jayson Tatum TS% / Nets TS% allowed in regular season) * 100
          (0.6146 / 0.5588) * 100 = 109.99 TS+
          
      Kobe Bryant faced a tougher defense relative to the league average scoring environment. After adjusting for this Kobe has a small advantage in efficiency (.55%)       rather than the original edge Jayson had (15.87%)
      
 #Adjusted Scoring Efficiency#

 Create adjusted playoff scoring rate
  1. Era Scoring Coefficient
   * Scraping NBA/ABA players' stats since 1974 (league avg. pts per possession for players)
   * Finding highest league avg. pts per 75 possessions. That is presently 2021 (16.72 PP75) 
   * Creating Era Scoring Coefficient for each season since 1974.

      Era Scoring Coefficient = max league avg pts per 75 possessions / current season pts per 75 possessions

      
          2004  
          16.72 (2021 avg PP75) / 15.33 (2004 avg PP75) = 1.09
          
          2022  
          16.72 (2021 avg PP75) / 16.68 (2022 avg PP75) = 1.002

      Be it changes in officiating or league-wide philosophy, scoring is easier in 2022 than it was in 2004 on a possession basis. This is thus accounted for in cross-       era comparisons. 

   * Adjust players' PTS per 75 possessions (PP75) by Era Scoring Coefficient


      
         2004 Kobe Bryant vs Spurs
         24.48 (Kobe's actual PP75) * 1.09 (Era Scoring Coefficient/Adjustment) = 26.68 (Era Adjusted Scoring Rate)
         
         2022 Jayson Tatum vs Nets
         27.15 (Jayson's actual PP75) * 1.002 (Era Scoring Coefficient/Adjustment) = 27.20 (Era Adjusted Scoring Rate)
      
  2. Opponent Scoring Coefficient
   * Scraping NBA/ABA teams' stats since 1974 (DefRtg [points allowed per 100 possessions])
   * Finding rDefRtg (DefRtg / league avg. DefRtg)
   * Creating Opponent Scoring Coefficient for each playoff series

      Opponent Scoring Coefficient = league avg. DefRtg / opponent DefRtg


      
          2004 Spurs =
          (2004 league avg. DefRtg) / (2004 Spurs DefRtg)
          102.89 / 94.1 (2004 Spurs DefRtg) = 1.09
          
          2022 Nets =
          (2022 league avg. DefRtg) / (2022 Nets DefRtg)
          111.95 / 112.8 (2004 Spurs DefRtg) = 0.99
          
      2022 Nets were a below league average defense (1% worse). 2004 Spurs were 9% better than league average (making them one of the best regular season defenses in         NBA history)
      

   * Adjust players' PTS per 75 possessions (PP75) by Opponent Scoring Coefficient


      
          2004 Kobe Bryant vs Spurs =    
          26.68 * 1.09 = 29.08
          
          2022 Jayson Tatum vs Nets =    
          27.20 * 0.99 = 26.93
      
   Consquently, after adjusting for era and opponent quality, Kobe Bryant's 2004 series vs the Spurs looks much more in line (or better) than the scoring stats generated by a modern player like Jayson Tatum.
   
   Adjusted Scoring:
   
   Kobe Bryant vs 2004 Spurs: 29.08 PP 75; 110.54 TS+
   
   Jayson Tatum vs 2022 Nets: 26.93 PP 75; 109.99 TS+
   
   Kobe Bryant's series--originally appearing far inferior to Jayson Tatum's--now looks superior.
