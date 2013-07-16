# xunit4gas

This experiment is too simple, just version 0.0.0.0....1.

[This presentation](http://www.slideshare.net/keiswd/google-apps-scripttdd) might be helpful to understand background of this code, if you really get interested in this.(but sorry only Japanese version is available.)

### Usage(Google Apps Script code)

    eval(UrlFetchApp.fetch("https://raw.github.com/remore/framework_experiment/master/framework_experiment.js").getContentText());
      
    framework_experiment.prototype._translate = function( string ){
      return string + "(" + LanguageApp.translate(string, "ja", "en") + ")";
    }

    framework_experiment.prototype.run = function(testcase){
      var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheets()[0];
      
      // specify where to write down the test results
      var i = sheet.getLastRow()+1;
      
      // write down the results to the spreadsheet;
      sheet.getRange("A"+i).setValue( new Date().toLocaleString());
      sheet.getRange("B"+i).setValue( Session.getActiveUser().getEmail());
      sheet.getRange("C"+i).setValue( this._translate(testcase) );
      sheet.getRange("D"+i).setValue( this.assertions.length);
      sheet.getRange("E"+i).setValue( this._countResult(true).sum);
      sheet.getRange("F"+i).setValue( this._countResult(false).sum);
      sheet.getRange("G"+i).setValue( this._translate( this._countResult(false).message));

    }

    function テストケースその１() {
      var test = new framework_experiment;
      test.assertEquals("a", "b", "aとbを入力した場合");
      test.assertEquals("a", "a", "aとaを入力した場合");
      test.assertEquals("c", "b", "cとbを入力した場合");
      test.run(arguments.callee.name);
    }

