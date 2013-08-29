# Javascript API
Javascript Connector for Agile

# Usage
You need to have an agile account to use the API

Create an account at http://www.agilecrm.com

###Putting the analytics JS code
Go to Admin Settings -> API & Analytics -> Analytics Code
Copy the 6 lines of code and put it in your webpage’s html (preferably in the head tag).
This code should be placed on all pages for which you need tracking.

###Using the API
Once this code is put, you can use various API calls below for analytics, and for pushing
contact data from website to Agile. 

#####Pushing contact data to Agile
When a website visitor fills a form with his email and other information, you can add the 
visitor as a contact in Agile, with the following method.
```html
_agile.create_contact({"email": "jim@example.com", "first_name":"Jim", 
		       "last_name":"Brown", "tags":"tag1, tag2"});
```
Email is mandatory and all other data is optional. The optional “tags” should be followed 
by a comma separated string of all tags you want to add to the contact being created.

#####Tracking a known person
Once you know the Email Id of the person, you can use the code below to give the same to Agile.
Agile stores the email id in the cookies and all subsequent visits by this person are tracked.
```html
_agile.set_email('jim@example.com');
```
Typically, once a visitor fills a form on your website, you should call  _agile.create_contact,
and also call _agile.set_email.
Once this is done, you will start getting real-time notifications on Agile whenever the contact
is on your website. You will also see his website visits in Timeline & Webstats tabs for that contact.

#####Scoring & segmenting contacts 
You can score your leads/contacts when they visit a particular web page using the call below
```html
_agile.add_score(10);
```
You can segment your contacts based on pages visited or options they choose on your website using
the code below. You can specify a list of tags.
```html
_agile.add_tag('tag1, tag2, tag3');
```
You can check more API calls under 
######Admin Settings > API & Analytics > Analytics Code

See [testcontact1.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact1.html) and [testcontact2.html](https://github.com/agilecrm/javascript-api/blob/master/testcontact2.html) for example implementations of all available API
