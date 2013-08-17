# Javascript API
Javascript Connector for Agile

# Usage
You need to have an agile account to use the API

Create an account at http://www.agilecrm.com

```html
<script type="text/javascript" src="https://YOUR_DOMAIN.agilecrm.com/stats/min/agile-min.js"></script>

<script type="text/javascript" >

// ------------SET YOUR ACCOUNT-----------------
_agile.set_account('YOUR_API_KEY', 'YOUR_DOMAIN');
        
// ------------SET YOUR CONTACT ----------------
_agile.set_email('YOUR_CONTACT_EMAIL');

// ------------SET TO TRACK PAGE VIEW-----------
_agile.track_page_view(callback);
        
// ------------ADD SCORE TO CONTACT SET ABOVE---
_agile.add_score(50, callback);
        
</script>
```
See [TestContact.html]() for example implementation 
