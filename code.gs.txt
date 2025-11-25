const ALPHA_VANTAGE_API_KEY = "5F1P6AIPU9FCWGBC"; // Get from https://www.alphavantage.co/
const ALPHA_VANTAGE_API_URL = "https://www.alphavantage.co/query?function=GLOBAL_QUOTE&symbol=";
const NEWS_API_KEY = "024bb6ea82564151af159a51414d2092"; // Get from https://newsapi.org/
const NEWS_API_URL = "https://newsapi.org/v2/everything?q=stock%20market&sortBy=publishedAt&apiKey=" + NEWS_API_KEY;

// Fetch stock prices from Alpha Vantage API
function updateStockPrices() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Stock_Data");
  var symbols = sheet.getRange("A2:A").getValues().flat().filter(String);
  
  if (symbols.length === 0) return;
  
  symbols.forEach((symbol, index) => {
    var url = ALPHA_VANTAGE_API_URL + symbol + "&apikey=" + ALPHA_VANTAGE_API_KEY;
    var response = UrlFetchApp.fetch(url);
    var data = JSON.parse(response.getContentText());
    
    Logger.log("Response for " + symbol + ": " + response.getContentText()); // Log API response

    if (data["Global Quote"]) {
      var stockData = data["Global Quote"];
      sheet.getRange(index + 2, 2).setValue(stockData["05. price"]); // Price
      sheet.getRange(index + 2, 3).setValue(stockData["10. change percent"]); // Change %
      sheet.getRange(index + 2, 4).setValue(new Date().toLocaleString()); // Last Updated
    } else {
      Logger.log("No data found for " + symbol);
    }
  });
}

// Fetch stock market news from NewsAPI
function updateStockNews() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Stock_News");
  var response = UrlFetchApp.fetch(NEWS_API_URL);
  var data = JSON.parse(response.getContentText());
  
  sheet.clear(); // Clear previous news
  sheet.appendRow(["Headline", "Source", "URL", "Published Date"]);
  
  data.articles.slice(0, 10).forEach((article, index) => {
    sheet.appendRow([article.title, article.source.name, article.url, article.publishedAt]);
  });
}

// Schedule automatic updates every hour
function createTriggers() {
  ScriptApp.newTrigger("updateStockPrices").timeBased().everyHours(1).create();
  ScriptApp.newTrigger("updateStockNews").timeBased().everyHours(1).create();
}

// Run once to set up triggers
function setup() {
  createTriggers();
}
