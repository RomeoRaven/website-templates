# website-templates

Various website templates and tools I develop or use

# Using

* Not entire sites
* Mostly single page resources for starter pages or specific types of pages and tools
  * Example: A starter page for rendering JSON data from Google Sheets

## Google Sheet JSON Parser

Ex. quotes.html

1. Create the spreadsheet in **Google Sheets**
1. Set the permissions via the **Share** button to **Anyone with link can view**
1. **File --> Publish to the web -->** (This is where you can select a specific sheet or entire spreadsheet) **Publish**
1. You will need the **spreadsheetID** and **worksheetID**
1. For the **spreadsheetID**, it is the part of the URL between `https://docs.google.com/spreadsheets/d/` and `/edit#gid=`
1. For a single sheet in a spreadsheet the **worksheetID** will be `od6`.
1. For multiple sheets you will need to get the specific **worksheetID**
1. Replace your **spreadsheetID** in this code block: `https://spreadsheets.google.com/feeds/worksheets/spreadsheetID/public/basic?alt=json` and paste into a browser
1. In the raw JSON output find the line that shows: `https://spreadsheets.google.com/feeds/list/spreadsheetID/worksheetID/public/basic`. Get the **spreadsheetID** from there
1. With the **spreadsheetID** and **worksheetID** use those in this URL: `https://spreadsheets.google.com/feeds/list/spreadsheetID/worksheetID/public/values?alt=json` to get the raw JSON
1. You can then parse it with a bit of jQuery like below:

```
<!-- Here's where the magic will appear -->
 <div class="results"></div> 
<!-- Here's where the magic happens -->
  <script>
   // ID of the Google Spreadsheet
   var spreadsheetID = "REPLACE_ME";
   var worksheetID = "REPLACE_ME"; // for the first sheet, default is 'od6'

    // Make sure it is public or set to Anyone with link can view 
    var url = "https://spreadsheets.google.com/feeds/list/" + spreadsheetID + "/" + worksheetID + "/public/values?alt=json";

   $.getJSON(url, function(data) {

      var entry = data.feed.entry;

    $(entry).each(function(){
      // Change line below with column headers: gsx$TITLE. Ex. column names are: quote, cite
      // You can also change the HTML tags used to wrap the data. Ex. <h1>, <p>, <dd>, <blockquote>, etc
      $('.results').prepend('<blockquote>'+this.gsx$quote.$t+'</blockquote><cite>'+this.gsx$cite.$t+'</cite><hr>');
    });
   });
  </script>
    ```
