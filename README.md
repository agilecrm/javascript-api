# Javascript API

Javascript Connector for Agile

# Usage

You need to have an agile account to use the API

Create an account at https://www.agilecrm.com

### Putting the analytics JS code

- Go to ***Admin Settings -> API & Analytics -> Analytics Code***

- Copy the 6 lines of code and put it in the body tag (preferably at the end) of your webpage's HTML.

![Finding Analytics Code] (https://raw.github.com/agilecrm/javascript-api/master/analytics_code.png)

- This code should be placed on all pages for which you need tracking.

### Using the API

- Once this code is put, you can use various API calls below for analytics, and for pushing contact data from website to Agile. 

#### Pushing contact data to Agile

When a website visitor fills a form with his email and other information, you can add the visitor as a contact in Agile, with the following method.

```javascript
var contact = {};

contact.email = "jim@example.com";
contact.first_name = "Jim";
contact.last_name = "Brown";
contact.company = "abc corp";
contact.title = "lead";
contact.phone = "+1-541-754-3010";
contact.website = "http://www.example.com";
contact.tags = "tag1, tag2";
contact.address = "{\"city\":\"new delhi\", \"state\":\"delhi\",\"country\":\"india\"}";

_agile.create_contact(contact, {
    success: function (data)
    {
        console.log("success callback");
    },
    error: function (data)
    {
        console.log("error callback");
    }
});
```
- Email is mandatory and all other data is optional. The optional “tags” should be followed by a comma separated string of all tags you want to add to the contact being created.

#### Setting a contact property

You can update a contact's property with the following call.

```javascript
property.name = "field_name";
property.value = "field_value";

_agile.set_property(property, {
    success: function (data)
    {
        console.log("success callback");
    },
    error: function (data)
    {
        console.log("error callback");
    }
});
```

- field_name can be one of *'first_name', 'last_name', 'email', 'phone', 'website', 'company', 'title', 'address'*.

- It can also be the name of a *Custom Field* that you defined.

- Some fields are multi-valued and have a ‘type’. For example, 'email' can be of type 'work'or 'personal'

- To set the type, you can pass the and additional key value pair in the json

```json
{
	"name": "field_name",
	"value": "field_value",
	"type": "field_type"
}
```
The list of possible 'type' for various fields are as mentioned below:

 - email \- work | personal

 - phone \- work | home | mobile | main | home fax | work fax |other

 - website \- website | skype | twitter | linkedin | facebook | xing | blog | google+ | flickr | github | youtube

 - address \- home | postal | office

#### Tracking website visitors
To track visitors on your website / application, you can use the call.

```javascript
_agile.track_page_view(
{
	success: function(data)
	{
		console.log("success callback");
	},
	error: function(data)
	{
		console.log("error callback");
	}
});
```
- But for this to work, you need to first let Agile know the email address of the website visitor.

- If you know the email address of the visitor (when the visitor fills the contact form) you should make the following call.

```javascript
_agile.set_email(visitor_email);
```
- Ideally, this needs to be done only once for each of your site visitors.

- Agile stores the email address in the browser cookies and uses it for all subsequent API calls made wherever email address is not provided explicitly.

- Typically, once a visitor fills a form on your website, you should call  ```_agile.create_contact```,
  and also call  ```_agile.set_email```.

- Once this is done, you will start getting real-time notifications on Agile whenever the contact is on your website.
  You will also see his website visits in Timeline & Webstats tabs for that contact.

#### Scoring & segmenting contacts 

You can score your leads/contacts when they visit a particular web page using the call below.

```javascript
_agile.add_score(10, {
	success: function(data)
	{
		console.log("success callback");
	},
	error: function(data)
	{
		console.log("error callback");
	}
});
```
You can segment your contacts based on pages visited or options they choose on your website using the code below. You can specify a list of tags.

```javascript
_agile.add_tag('tag1, tag2, tag3', {
	success: function(data)
	{
		console.log("success callback");
	},
	error: function(data)
	{
		console.log("error callback");
	}
});
```
- **Note**: These methods will work only if you have called ```_agile.set_email```  method earlier to store the email address of the contact in the cookie.
Please check the **Tracking website visitors** section for more information on ```_agile.set_email``` method. 

#### Adding Note, Task or Deal to contact

You have set email, using ```_agile.set_email``` before calling the below API calls.

To add a note to contact

```javascript
_agile.add_note(
{
	"subject": "Test Note",
	"description": "This is a test note"
}, {
	success: function(data)
	{
		console.log("success callback");
	},
	error: function(data)
	{
		console.log("error callback");
	}
});
```

To add task to contact

```javascript
_agile.add_task(
{
	"type": "MEETING",
	"priority_type": "HIGH",
	"subject": "This is a test task"
}, {
	success: function(data)
	{
		console.log("success callback");
	},
	error: function(data)
	{
		console.log("error callback");
	}
});
```

You can add deal to contact, here close date is specified as epoch time.

```javascript
var deal = {};
deal.name = "Test Deal";
deal.description = "This is a test deal";
deal.expected_value = "100";
deal.milestone = "won";
deal.probability = "5";
deal.close_date = "1376047332";

_agile.add_deal(deal, {
    success: function (data)
    {
        console.log("success callback");
    },
    error: function (data)
    {
        console.log("error callback");
    }
});
```

- **Note**: These methods will work only if you have called  ```_agile.set_email```  method earlier to store the email address of the contact in the cookie.
Please check the **Tracking website visitors** section for more information on ```_agile.set_email``` method. 


See [create_contact.html](https://github.com/agilecrm/javascript-api/blob/master/create_contact.html) and [all.html](https://github.com/agilecrm/javascript-api/blob/master/all.html) for example implementations of all available API
