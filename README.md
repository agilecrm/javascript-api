# javascript-api

* * *

Javascript Connector for Agile

# Usage

* * *

TestAgile.html

`
<!DOCTYPE html>
<html>
  <title> Test Agile CRM JavaScript API </title>
  <head>
    <script type="text/javascript" src="https://YOUR_DOMAIN_NAME.agilecrm.com/stats/min/agile-min.js"></script>
  </head>
  <body>
    <script type="text/javascript" >

      // ------------SET YOUR ACCOUNT-----------------
      _agile.set_account('YOUR_API_KEY', 'YOUR_DOMAIN_NAME');

      // ------------SET TRACK PAGE VIEW--------------
      _agile.track_page_view(callback);

      // ------------SET CONTACT EMAIL----------------
      _agile.set_email('YOUR_CONTACT_EMAIL');

      // ------------EXAMPLE API CALL-----------------

      // ------------ADD SCORE TO CONTACT SET---------
      _agile.add_score(50, callback);

    </script>
  </body>
</html>
`

See [TestContact.html]()for more example
