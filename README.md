# kikibot™
## Documentation for [Shell bot](https://github.com/kayleecragg/kikibot?tab=readme-ov-file#shell-bot) ([click here](https://colab.research.google.com/drive/1jFuPN-3OMjf6p990YB8j_fsw3-JhViI0?usp=sharing))

### [Shell bot:](https://colab.research.google.com/drive/1jFuPN-3OMjf6p990YB8j_fsw3-JhViI0?usp=sharing)

#### First section: Downloading/installing necessary packages/libraries
* Downloading JSON key from dropbox
  * <sub> *JSON key is required for using google sheets api* </sub>
* Installing/upgrading gspread libraries + Node.js
  * <sub> *Gspread is required for using google sheets api* </sub>
  * <sub> *Node.js is required for using Puppeteer* </sub>
* Installing Puppeteer
  * <sub> *Puppeteer is required to scrape Australian open's website as it is rendered with React* </sub>

#### Second section: Scraping Australian Open webpage
* Enter the URL to Scrape
  * <sub> **Each day needs a separate input** </sub>
  * <sub> *The link will look something like this https://ausopen.com/schedule#!32076* </sub>
* Writing scraping instructions in index.js file
   * <sub> *Writing instructions into a Node.js file that will open up a browser, go to the link you input, and scrape the whole webpage into a html file named 'Scraped.html'* </sub>
* Running index.js file
   * <sub> *Actually running the Node.js file so that the Scraped.html file will be created and stored for future use* </sub>


#### Third section: Transporting data scraped from Aus open website to Google Sheets/ AKA Shells
* Invoking Variables
  * <sub> ***Minimal input needed** (do this before you run the whole section, but should only need to do once per competition)* </sub>
  * <sub> *Just has things like credential file location (static), sheet_id (once per comp), template_sheet_id (once per comp), template_worksheet_name (once per comp), sheet_range (static, unless programming adds additional columns to the sheet..* :cold_sweat:	*)* </sub>
* Parsing data from HTML file to Google Sheets
  * <sub> *Extracting raw data from the scraped html file like Round, Time, First Player, Second Player, Gender, Court name, ranks of the players, and match numbers and writing them into a data sheet in google sheets (which google sheets can be specified in the invoking section), interating the data over each match until there are none left.* </sub>
* Parsing the existing times
  * <sub> *Takes the existing times from **certain matches** and assigns them to the respective matches in the google sheets.* </sub>
  * <sub> *Why is this separate? Because this feature was implemented later on in the development stage and it got too complicated to combine into previous script* </sub>
* Calculating the rest of the times
  * <sub> *As there is now a bunch of matches without times right now, this script fills in the matches without times assigned to them by calculating the times between them. For example, if the previous match was a **Men's Singles** match, then it would add 3 hours to the previous start time, for anything else it would add 2 hours to the previous start time, skipping over matches with already assigned times. If there time that was assigned for a new court then it would interate over it.* </sub>
* Setting up initial Day from template
  * <sub> *This script copies over the initial two match structures from the template worksheet into a new worksheet named; 'Day (Day number)'.* </sub>
  * <sub> *Why is this separated from the rest? Because the first match's structure can't be duplicated without causing the error which stems from vlookup.* </sub>
<img width="808" alt="image" src="https://github.com/kayleecragg/kikibot/assets/70317319/4b0217f6-9c09-491f-ae73-e0e7e1cbb454">


* Copying over from match template based on amount of matches
  * <sub> *With the first match's structure out of the way, this section of the script duplicates the second match's structure as many times as there are matches that exist for the day.* </sub>
* Copying over match data from data sheet
  * <sub> *Now that the structure for all the existing matches are in, it's time to write in the match data column by column. (mostly from the data sheet)* </sub>
  * <sub> ***Why is this done secondly and not concurrently to the previous step? Wouldn't it be more efficient/faster?*** </sub>
  * <sub> *Because the data validation/formatting needs to pre-exist before the match data is inputted, it is safer to have all of the match structure already in the sheet before inputting any match data* </sub>

#### Ignore misc section
Just contains scraper for Aus Open players section (will only be needed once per competition).

### [Preimages bot:](https://colab.research.google.com/drive/1M2tbZjuFYC1Hk58EXUIWqs9gCeemUnln#scrollTo=m-g0DkAkbQfy)

#### First section: Downloading/installing necessary packages/libraries
* Downloading JSON key from dropbox
  * <sub> *JSON key is required for using google sheets api* </sub>
* Installing/upgrading gspread libraries
  * <sub> *Gspread is required for using google sheets api* </sub>
* Installing Selenium
  * <sub> *Selenium is required to interact with Stan's tile creator tool as it is rendered with Javascript* </sub>

#### Second section: Prepromote time
* Downloading Flags folder from Dropbox
  * <sub> *A Flags folder that contains all every flag from every country is downloaded from dropbox so that it can be used locally in the colab environment* </sub>

* Doing the prepromotes
  * <sub> *This section of code automates the creation and downloading of Live & Upcoming tiles and Post match logo cards for the Australian Open using a headless Chrome browser and data from the 'Tilebot' worksheet that was extracted from the Shell creation processs in Google Sheets. It involves the following steps:* </sub>

  * <sub> ***Setting Up Environment and Browser:** Imports necessary libraries, sets up a folder called ‘Prepromotes’, where all the pre image files and folders will be downloaded into, and configures headless Chrome browser options for automation. (Necessary for colab environment where there is no GUI* </sub>

  * <sub> ***Google Sheets API Integration:** Initializes Google Sheets API to fetch data from the 'Tilebot' worksheet in a specified Google Sheet, which contains details like player names, countries, match dates, and rankings.* </sub>

  * <sub> ***Web Automation with Selenium:** The script uses Selenium WebDriver to interact with [Stan's tile creator tool](https://thelivecms.prod.streamco.cloud/tile-creator/). It navigates through the tool's interface, selecting options based on the data fetched from the Google Sheet, such as the type of match, player details, and country flags.* </sub>

  * <sub> ***Input Handling and Data Entry:** The script inputs data into the web tool, including player names, rankings, and match details. It also handles file uploads for country flags and manages exceptions and timeouts.* </sub>

  * <sub> ***Downloading and Organizing Files:** After inputting all the data, the script triggers the download of zip files containing the tiles. It waits for downloads to complete, extracts the zip files, and organizes the extracted files into specific named folders based on the names of the players and the match type (singles or doubles).* </sub>

  * <sub> ***Finalization and Clean-Up:** The script compresses the organized folders into a zip file and triggers its download. Finally, it cleans up any temporary zipped files and closes the browser session.* </sub>
  
<!-- * <sub> ***[Line by line explanation](https://github.com/kayleecragg/kikibot/tree/main/preimages#readme)*** </sub> -->

#### Issues and recommendations:

Due to google sheets api request rate limits, the script is unfortunately limited to about **300 requests per minute** - otherwise it crashes. 
So a delay has been implemented into these two sections of code so that it runs within the boundaries of the rate limit.

* Copying over from match template based on amount of matches
    * <sub> ***Delays:** In this script, a 3.5 second delay has been implemented for every iteration per match. Total time taken about 9 minutes for 64 matches. (5 min 30 seconds without delay)* </sub>

* Copying over match data from data sheet
    * <sub> ***Delays:** In this script, a 3 second delay has been implemented for every iteration per match. Total time taken about 18 minutes for 64 matches. (14 min 40 sec without delay)* </sub>

**Solution:** One of the ways this can be solved is by **making an api key with (insert parent company here) account and then request a quota limit increase**.

Because the request is coming from an organisation instead of an individual, it wouldn't be denied. (Coming from personal experience of having a quota limit increase request denied)

**Reason solution hasn't been implemented yet:** Unable to make an api key with a (insert our parent company here) account as do not have necessary permissions. 

**Another note:** Pre images doesn't need to wait for these to finish to begin, (tilebot sheet can be made directly from data sheet so it can be made as soon as data sheet is ready, pre the 20 minutes shell creation time) so they can be worked on while the shells are being generated.
