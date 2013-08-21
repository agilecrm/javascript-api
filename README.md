# Javascript API
Javascript Connector for Agile

# Usage
You need to have an agile account to use the API

Create an account at http://www.agilecrm.com

Place the following script in the head tag of you webpage.

```html
<script type="text/javascript" 

src="https://YOUR_DOMAIN.agilecrm.com/stats/min/agile-min.js"></script>

<script type="text/javascript" >

// ------------SET YOUR ACCOUNT-----------------
_agile.set_account('YOUR_API_KEY', 'YOUR_DOMAIN');
        
// ------------SET YOUR CONTACT ----------------
_agile.set_email('YOUR_CONTACT_EMAIL');

</script>
```
See [testcontact1.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact1.html) and [testcontact2.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact2.html) for example implementations
