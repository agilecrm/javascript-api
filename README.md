# Javascript API

Javascript Connector for Agile

# Usage

You need to have an agile account to use the API

Create an account at http://www.agilecrm.com

### Putting the analytics JS code

Go to ***Admin Settings -> API & Analytics -> Analytics Code***

Copy the 6 lines of code and put it in the body tag (preferably at the end) of your webpage's HTML.

This code should be placed on all pages for which you need tracking.

### Using the API

Once this code is put, you can use various API calls below for analytics, and for pushing contact data from

website to Agile. 

#### Pushing contact data to Agile

When a website visitor fills a form with his email and other information, you can add the visitor as a contact in

Agile, with the following method.

```javascript
_agile.create_contact({"email": "jim@example.com",
		       		   "first_name" : "Jim",
		       		   "last_name" : "Brown",
		       		   "company" : "abc corp",
		       		   "tags" : "tag1, tag2"},
		       		   callback);
```
Email is mandatory and all other data is optional. The optional “tags” should be followed by a comma separated

string of all tags you want to add to the contact being created.

#### Tracking website visitors
To track visitors on your website / application, you can use the call.

```javascript
_agile.track_page_view(callback);
```
But for this to work, you need to first let Agile know the email id of the website visitor.

If you know the email id of the visitor (when the visitor fills the contact form) you should make the following call.

```javascript
_agile.set_email(visitorEmailId);
```
Ideally, this needs to be done only once for each of your site visitors.
Agile stores the email id in the browser cookies and uses it for all subsequent API calls made wherever email ID is not provided explicitly.

Once you know the Email Id of the person, you can use the code below to give the same to Agile.
Agile stores the email id in the cookies and all subsequent visits by this person are tracked.

```javascript
_agile.set_email('jim@example.com');
```
Typically, once a visitor fills a form on your website, you should call   ***_agile.create_contact***,
and also call  ***_agile.set_email***.

Once this is done, you will start getting real-time notifications on Agile whenever the contact is on your website.
You will also see his website visits in Timeline & Webstats tabs for that contact.

#### Scoring & segmenting contacts 

You can score your leads/contacts when they visit a particular web page using the call below.

```javascript
_agile.add_score(10, callback, optional_contact_email);
```
You can segment your contacts based on pages visited or options they choose on your website using the code below.

You can specify a list of tags.

```javascript
_agile.add_tag('tag1, tag2, tag3', callback, optional_contact_email);
```
The email id of the contact is required if you are not calling ***_agile.set_email*** method before any of these calls.

If that is done, the email parameter is optional.

Please check the ***Tracking website visitors*** section for more information on ***_agile.set_email*** method. 

#### Adding Note, Task or Deal to contact

Email is optional if you have set email above, using ***_agile.set_email***, else provide email to the API functions below.

To add a note to contact

```javascript
_agile.add_note({"subject":"test", "description":"note"},
			      callback,
			      optional_contact_email);
```

To add task to contact

```javascript
_agile.add_task({"type":"MEETING", "priority_type":"HIGH", "subject":"test"},
		 		 callback,
		 		 optional_contact_email);
```

You can add deal to contact, here close date is specified as epoch time.

```javascript
_agile.add_deal({"name":"Test Deal",
			     "description":"testing deal",
			     "expected_value":"100",
			     "milestone":"won",
			     "probability":"5", 
			     "close_date":"1376047332"},
			      callback,
			      optional_contact_email);
```

The email id of the contact is required if you are not calling ***_agile.set_email*** method before any of these calls.

If that is done, the email parameter is optional.

Please check the ***Tracking website visitors*** section for more information on ***_agile.set_email*** method

#### Adding a custom field entity to contact

If custom field entity with the same name is already related to contact, then it will be replaced.

```javascript
_agile.add_property({"name" : "custom_field_name",
		     		 "value" : "custom_field_value",
		     		 "type" : "CUSTOM"},
		     		  callback,
		     		  optional_contact_email);
```
The custom_field_name should be the name given while defining the custom field in Agile CRM

See [testcontact1.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact1.html), [testcontact2.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact2.html) and [testcontact3.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact3.html) for example implementations of all available API
