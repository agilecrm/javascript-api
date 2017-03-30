Agile CRM JavaScript API 
=================

[Agile CRM] (https://www.agilecrm.com/) is a new breed CRM software with sales and marketing automation.

Table of contents
---------------

**[API's details](#setting-api--analytics)**
  * [Setting API & Analytics](#setting-api--analytics)

**[Basic usage (Things to know)](#basic-usage-things-to-know)**
  * [1 Tracking Contacts](#1-tracking-contacts)
  * [2 Tracking page views](#2-tracking-page-views)
  * [3 Email override](#3-email-override)

**[API usage (Things to know)](#api-usage)**
  * [1 Tracking website visitors](#1-tracking-contacts)
    * [1.1 Setting Email of the page visitor](#11-setting-email-of-the-page-visitor)
    * [1.2 Get Email](#12-get-email)
    * [1.3 Tracking across multiple sites](#13-tracking-across-multiple-sites)

**[2 Contact](#2contact)**
  * [1 Create contact](#21-create-contact)
  * [2 Get contact](#22-get-contact)
  * [3 Update contact](#23-update-contact)
  * [4 Set contact property](#24-set-contact-property)
  

**[3 Tags](#3tags)**
  * [1 Add tags](#31-add-tags)
  * [2 Get tags](#32-get-tags)


**[4 Score](#4score)**
  * [1 Add score](#41-add-score)
  * [2 Get Score](#42-get-score)

**[5 Task](#5task)**
  * [1 Add Task](#51-add-task)

**[6 Note](#6note)**
  * [1 Add Note](#61-add-note)

**[7 Deal](#7deal)**
  * [1 Add Deal](#71-add-deal)

**[8 Company](#8company)**
  * [1 Create Company](#81-create-company)

## API's details
### Setting API & Analytics
- You may access the analytics code from ***Admin Settings -> API & Analytics -> Analytics Code***
- Copy the 6 lines of script and paste in it in your webpage's HTML just before the ```</BODY>``` tag, for which you need API methods and / or tracking.

![Finding Analytics Code] (https://github.com/agilecrm/javascript-api/blob/master/analytics_code_new.jpg)



## Basic usage (Things to know)
### 1. Tracking contacts: 

```javascript
_agile.set_email('visitor@youwebsite.com')
```

Agile tracks contacts automatically from the emails clicks if you have tracking enabled (track and push) in your outbound emails in the campaigns or one-on-one emails. This step is not required unless you have a landing form or offer custom login for your application. Agile sets a cookie for a validity is for one full year. You do not have to do set_email for every visit. 

### 2. Tracking page views

Agile tracks the page views for each session for each contact. The visitor is tracked as anonymous unless a set_email is done.  The correct step is to set_email and then track_page_view. Anonymous users are tracked and backtracked when a user email is added using set_email. All the old page views are attributed to the new email address. This is often the case where the user browses many sessions and then signups for your product or provides the email in the landing pages during product download.

### 3. Email override

If a new email address is provided, Agile treats them as a new user and the sessions are tracked for the new user. The old page views are still valid for the old email and can be accessed from Agile dashboard.

## API usage

API can be used to sync data from your website/app to Agile CRM.

**Note: All API calls are Asynchronous. To execute multiple API calls simultaneously, you need to place the subsequent API call in the success callback of the previous call.**


### 1.Tracking website visitors

You can track the visitors to your website by adding the 6 lines of code you see above - which includes the ```_agile.track_page_view``` call. 
But for this to work, you need to call ```_agile.set_email```, with the email address of site visitor (when he fills a form etc).
Typically, once a visitor fills a form on your website, you should call ```_agile.create_contact``` and ```_agile.set_email```.

Once this is done, you will start getting real-time notifications on Agile whenever the contact is on your website. You will also see website visits in Timeline and in Webstats tab for that contact.
Agile also tracks anonymous visitors (i.e visitors for whom you have not called ```_agile.set_email```), and once the vistor fills a form and you do a create_contact, set_email, all the previous web history is attached to the contact.

#### 1.1 Setting Email of the page visitor

To set the tracking cookie for the visitor, use the API call below

- Parameters : contact email

```javascript
_agile.set_email("visitor@youwebsite.com");
```

**Note: This call is mandatory for API calls to work. They all use the email address set here to add / update the contact info and associated data.**

#### 1.2 Get Email

To get the email of visitor (whose email was already set earlier)

- Parameters : callback object

```javascript
_agile.get_email({
    success: function(data){
        console.log(data.email);
    },
    error: function(data){
        console.log(data.error);
    }
})
```
- Success data :

visitor@youwebsite.com

#### 1.3 Tracking across multiple sites
It is possible to track a vistor on multiple subdomains of your site. i.e www.mysite.com and abc.mysite.com. 
The API call _agile.set_tracking_domain ('mysite.com') allows this. This call should be placed in between _agile.set_account() and  _agile.track_page_view() calls. 

When this is used, visitor email that is set using set_email on one of your sites, can be accessed using the get_email on the other sites (subdomains).

### 2.Contact
#### Format of the contact object is as follows

#### 2.1 Create Contact

To create contact.

Contact properties are :

**first_name**, **last_name**, **email**, **company**, **website**, **title**, **tags**, **phone**, **address**.

- Parameters : contact, callback object (optional)

```javascript
var contact = {};
contact.email = "contact@test.com";
contact.first_name = "Test";
contact.last_name = "Contact";
contact.company = "abc corp";
contact.title = "lead";
contact.phone = "+1-541-754-3010";
contact.website = "http://www.example.com";
var address = {"city":"new delhi", "state":"delhi", "country":"india"};
contact.address = JSON.stringify(address);
contact.tags = "tag1, tag2";

// Custom fields can be added to contact object as
contact.status = "incomplete";
contact.custom_id = "EN001C";

_agile.create_contact(contact, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data :
**data** parameter of the success callback function is the created <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

```javascript
{success: function(data){
	console.log(data);
}}
```
- Error data :
**data** parameter of the error callback function provides the error message.

```javascript
{error: function(data){
    console.log(data.error)
}}
```

some of the common error messages are as follows
    
    - Duplicate found for "test@contact.com"
    - Invalid API key
    - Invalid parameter
    - Contacts limit reached
    - API key missing

#### 2.2 Get Contact

Contact can be searched (based on email already set using ```_agile.set_email```).

- Parameters : contact email, callback object (optional)

```javascript
_agile.get_contact("contact@test.com", {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data :

**data** parameter of the success function returns the <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of the error callback provides the error message for failed API call.
```javascript
error: function(data){
    console.log(data.error);
}
```

some of the common error messages are as follows

    - Contact not found
    - Invalid API key
    - API key missing

#### 2.3 Update Contact

Updates the contact with given JSON data (based on email already set using ```_agile.set_email```).

- Parameters : contact data, callback object (optional)

```javascript
_agile.update_contact({
    "title": "lead",
    "website": "http://www.example.com"
}, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data :

**data** parameter of success callback is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback provides the error message object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

#### 2.4 Set Contact Property

To add new or update existing contact property (*first_name*, *last_name*, *title*, *company*, *website*, *phone*, *address*) or any CUSTOM contact property (based on email already set using ```_agile.set_email```).


- Parameters : property, callback object (optional)

```javascript
var property = {};
property.name = "abc account";
property.value = "premium";

_agile.set_property(property, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```

Some fields are multi-valued and have **type**. For example, *email* can be of type *work* or *personal*. 

To set the type, property object must have an additional attribute **type** like
```property.type = "work";```. If you pass JSON directly then you need to include an additional key value pair like

```json
{
    "name": "field_name",
	"value": "field_value",
	"type": "field_type"
}
```
The list of possible **type** for various fields are as mentioned below:
- email \- work | personal

- phone \- work | home | mobile | main | home fax | work fax |other

- address \- home | postal | office
- website \- website | skype | twitter | linkedin | facebook | xing | blog | google+ | flickr | github | youtube

Below is an example API call to add / replace (if exists) office address to a contact (based on email already set using ```_agile.set_email```).

```javascript
var property = {};
property.name = "address";
var address = {"city":"new delhi", "state":"delhi", "country":"india"};
property.value = JSON.stringify(address);
property.type = "office";

_agile.set_property(property, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of success callback is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback provides the error message object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```
### 3.Tags

#### 3.1 Add Tags

Adds tags to the contact (based on email already set using ```_agile.set_email```). 

- Parameters : tags, callback object (optional)

```javascript
_agile.add_tag('tag1, tag2, tag3, tag4, tag5', {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback method is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error:"Invalid API key"}
{error: "Contact not found"}
```

#### 3.2 Get Tags

Gets the tags associated with the contact (based on email already set using ```_agile.set_email```).

- Parameters : callback object (optional) 

```javascript
_agile.get_tags({
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback is the array of tags.

```javascript
["tag4", "tag5"]
```
- Error data :

**data** parameter of error callback is the error object.

```javascript
{error:"Invalid API key"}
{error: "Contact not found"}
```
### 4.Score
#### 4.1 Add Score

Add score to contact (based on email already set using ```_agile.set_email```).

- Parameters : score, callback object (optional)

```javascript
_agile.add_score(50, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback method is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

#### 4.2 Get Score

Get score associated with contact (based on email already set using ```_agile.set_email```).

- Parameters : callback object (optional)

```javascript
_agile.get_score({
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback is the updated score.

```javascript
10
```
- Error data :

**data** parameter of error callback is the error object. Message can be accessed as data.error

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```


### 5.Task
#### Format of the task object is as follows

#### 5.1 Add Task

Add task to contact (based on email already set using ```_agile.set_email```).

Task object has the following attributes

**type** with values as *CALL* | *EMAIL* | *FOLLOW_UP* | *MEETING* | *MILESTONE* | *SEND* | *TWEET*,

**priority_type** with values as *HIGH* | *NORMAL* | *LOW*, **subject** the subject of task and

**due** which is the due date for the task.

- Parameters : task, callback object (optional)

```javascript
var task = {};
task.type = "MEETING";
task.priority_type = "HIGH";
task.subject = "Sample Task";
task.due = "1376047332";	// Epoch time

_agile.add_task(task, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback method is the created <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-task-object-is-as-follows' target='_blank'>task object</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

### 6.Note
#### Format of the note object is as follows

#### 6.1 Add Note

Creates a new note (based on email already set using ```_agile.set_email```). Note object has the properties **subject** the subject of the note, and **description** note description.

- Parameters : note, callback object (optional)

```javascript
var note = {};
note.subject = "Test Note";
note.description = "This is a test note";

_agile.add_note(note, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback is the created <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-note-object-is-as-follows' target='_blank'>note object</a>.

- Error data :

**data** parameter of the error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

### 7.Deal
#### Format of the deal object is as follows

#### 7.1 Add Deal

Add deal to contact (based on email already set using ```_agile.set_email```). Deal object has the properties **name** name of the deal, **description** deal description, **expected_value**, **milestone**, **probability** and **close_date**.

- Parameters : deal, callback object (optional)
- Note : Deal milestones name is case sensitive. It should be match the name with this url (https://{domain}.agilecrm.com/#milestones)

```javascript
var deal = {};
deal.name = "Test Deal";
deal.description = "This is a test deal";
deal.expected_value = "10000";
deal.milestone = "Won";
deal.probability = "95";
deal.close_date = "1376047332";		// Epoch time

_agile.add_deal(deal, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data : 

**data** parameter of the success callback is the created <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-deal-object-is-as-follows' target='_blank'>deal object</a>.

- Error data :

**data** parameter of the error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

### 8.Company
#### Format of the company object is as follows

#### 8.1 Create Company

Add company as contact. Company object has the properties **name**, **phone**, **url**, **address**.

- Parameters : company, callback object (optional)

```javascript
var company = {};
company.name = "abc inc";
company.phone = "+1-541-754-3010";
company.url = "http://www.abc-inc.com";
var address = {"city":"new delhi", "state":"delhi", "country":"india"};
company.address = JSON.stringify(address);

_agile.create_company(company, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
- Success data :
**data** parameter of the success callback function is the created <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-company-object-is-as-follows' target='_blank'>company object</a>.

```javascript
{success: function(data){
	console.log(data);
}}
```
- Error data :
**data** parameter of the error callback is the error object.

```javascript
{error: function(data){
    console.log(data.error)
}}
```
some of the common error messages are as follows
    
    - Duplicate found for "http://www.abc-inc.com"
    - Invalid API key
    - Invalid parameter
    - Contacts limit reached
    - API key missing

## More

You may refer to [create_contact.html](https://github.com/agilecrm/javascript-api/blob/master/create_contact.html) and [all.html](https://github.com/agilecrm/javascript-api/blob/master/all.html) for example implementation of multiple API methods simultaneously. 
