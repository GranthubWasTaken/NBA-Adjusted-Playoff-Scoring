# NBA-data
# Functions Scraping Basketball-Reference/Tallying Multi-Year Stretches
 scrape player data from basketball-reference since 1952 (excluding 1954*) for
   * player URL's
   * shooting location data (only extending back to 1997)
   * per 100 possession data
   * advanced data
   * play-by-play data (only extending back to 1997)
   
 find multi-year stretches of statistical values for players (and export csv file containing each unique stretch of data for each player) other functions to organize and manipulate data in more digestible or interesting formats.
 
     such as, comparing change in statistical values for each player between regular season and postseason
     e.g. 5 year change in regular season and postseason PTS per 75 (pace-adjusted scoring rate) and TS+ (era/opponent adjusted scoring efficiency) for Kevin Durant from 2015-19.
     
     
     
 # Creating Era/Opponent Adjusted Playoff Scoring
 
  Reasoning: Players' raw scoring stats cannot be accurately compared across various periods of NBA history nor various qualities of opponent defense.
  

    
        2004 Kobe Bryant vs San Antonio Spurs: 24.48 Pts per 75 possessions (PP75); .5304 TS%
        
        2022 Jayson Tatum vs Brooklyn Nets: 27.15 Pts per 75 possessions (PP75); .6146 TS%
      
  Jayson Tatum has higher scoring rate and efficiency. Both conventional measures of scoring effectiveness. Is this evidence Tatum had a better scoring series?
  
  Basic theory on adjusting for Opponent Quality/Era: 
  
  #Adjusted Efficiency#
  
  1. Scraping NBA teams' defensive stats since 1974 (TS% allowed [points per possession allowed]).
  2. Scraping player URL's since 1952 (> 1000 MP) and using their basketball-reference player pages' organized playoff series data table to total playoff scoring.
   * Dividing a player's TS% by their playoff opponent's regular seasons TS%; series-by-series
   * Effectively, player efficiency in playoff series / opponent efficiency allowed in regular season


      
          2004 Kobe Bryant vs. Spurs
          (Kobe Bryant TS% / Spurs TS% allowed in regular season) * 100
          (0.5304 / 0.4798) * 100 = 110.54 TS+
          
          2022 Jayson Tatum vs. Nets
          (Jayson Tatum TS% / Nets TS% allowed in regular season) * 100
          (0.6146 / 0.5588) * 100 = 109.99 TS+
          
      Kobe Bryant faced a tougher defense relative to the league average scoring environment. After adjusting for this Kobe has a small advantage in efficiency (.55%)       rather than the original edge Jayson had (15.87%)
      
 #Adjusted Scoring Rate#

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

      Whether it be due to changes in officiating or league-wide philosophy, scoring is easier in 2022 than it was in 2004 on a possession basis. This is thus accounted for in cross-era comparisons. 

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
   
# Estimating Possession Data & Opponent TS% Allowed from 1952-73

  Reasoning: Pace is often normalized for sake of comparing players on slower/faster teams or slower/faster era's. Thus we can see scoring ability on a possession basis. Official pace data does not exist before 1974. This data may be estimated from 1952-73 (using stats that are available) so that cross-era comparisons may be made. With estimated pace we can estimate what DefRtg's opponents' allowed, and using estimated DefRtg we can go one step further and estimate what TS% teams allowed to their opponents (based on the correlation between rDefRtg and rTS% allowed from 1974-2022).
  
  These numbers come with a hefty grain of salt but their estimated values are better than none at all.
  
  1. Estimating Pace: Pace is estimated with the formula developed by Ben Taylor (https://twitter.com/ElGee35)
  2. Scaling PTS allowed per game into PTS allowed per 100 possesions (defRtg) for each team in the NBA from 1952-73
  3. Finding correlation between rDefRtg (DefRtg / league avg. DefRtg) and rTS% allowed (TS% allowed / league avg. TS% allowed) from 1974-2022.
  * We have a huge sample of definite data from 1974-2022. Using this data we can see the correlation between rDefRtg and rTS% allowed and apply that correlation on the estimated rDefRtg's we have from 1952-73.
  * Thus the logic is using an estimated rDefRtg to create an estimated rTS% allowed.
  * Supporting data for opponent-adjusted playoff scoring numbers is two layers of estimation deep. Numbers are not concrete like 1974-2022 but without complete films of every game from 1952-73 (which doesn't exist) the concrete numbers are forever lost from history. Estimated data is best available option to compare players from 1952-73 with pace and opponent quality factored in.
  4. We can now use same era/opponent adjusted logic used on 1974-2022 playoff data on 1952-73 estimated playoff data. The one exception thus far being the 1954 playoffs as the logic has not been written to handle the initial round robin playoff format the NBA used as a one-off in 1954.

Issues with 1952-73 data: MP, FGA, and FTA, all critical to creating adjusted efficiency, are often missing from NBA boxscores in the 1950's and 1960's. Basketball-Reference uses what I believe is an estimated total number for each players' playoffs. I utilized these numbers whenever MP, FGA, or FTA were missing from a players' series data table. I divided them evenly amongst the number of games played in each series. If there are 240 MP missing amongst 2 series, and 1 series had 4 games and the other series had 5 games, I gave the 4 game series 106.67 MP (240 * (4 Game Series / 9 Game total)) and the 5 game series 133.33 MP (240 * (5 Game Series / 9 Game total)). In reality there is no possible way to definitely know the real distribution of missing MP/FGA/FTA data samples; dividing them evenly among the number of missing games was my natural solution.
