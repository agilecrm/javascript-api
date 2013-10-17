JavaScript API
==============
JavaScript Connector for Agile

You need to have an agile account to use the API.

Create an account at https://www.agilecrm.com

# Important Info
#### Setting API & Analytics
- You may access the analytics code from ***Admin Settings -> API & Analytics -> Analytics Code***
- Copy the 6 lines of script and paste in it in your webpage's HTML just before the ```</BODY>``` tag, for which you need API methods and / or tracking.

![Finding Analytics Code] (https://raw.github.com/agilecrm/javascript-api/master/analytics_code.png)

#### NOTE
- Contact email must be set once using ```_agile.set_email``` method before calling the API, to ensure that email is stored in the cookie and is available for API execution.
- To execute multiple API simultaneously, you need to place the API call in the success callback of the previous API call.
- Format of callback is 

```javascript
{
    success: function(data){
        console.log("success callback here");
    },
    error: function(data){
        console.log("error callback here");
    }
}
```
Callback is optional parameter for API methods.

#Usage

###0.Tracking website visitors

The API call for tracking is ```_agile.track_page_view(callback)``` it is included by default in the analytics script that you need to embed in the html of your webpage to enable API methods / tracking in general.

- But to begin tracking a particular contact you need to call ```_agile.set_email```, with the email address of the contact.

- Typically, once a visitor fills a form on your website, you should call ```_agile.create_contact``` and ```_agile.set_email```.

- Once this is done, you will start getting real-time notifications on Agile whenever the contact is on your website. You will also see website visits in Timeline and in Webstats tab for that contact.

###1.Email
#### 1.1 Set Email

To set contact email, for API execution and to begin tracking. API calls will not function if contact email is not set, so ```_agile.set_email``` is mandatory before executing the API.

- Parameters : contact email

```javascript
_agile.set_email("contact@test.com");
```
###2.Contact
#### 2.1 Create Contact

To create contact. Contact properties are **first_name**, **last_name**, **email**, **company**, **website**, **title**, **tags**, **phone**, **address**.

- Parameters : contact, callback

```javascript
var contact = {};
contact.email = "contact@test.com";
contact.first_name = "Test";
contact.last_name = "Contact";
contact.company = "abc corp";
contact.title = "lead";
contact.phone = "+1-541-754-3010";
contact.website = "http://www.example.com";
contact.address = "{\"city\":\"new delhi\",\"state\":\"delhi\",\"country\":\"india\"}";
contact.tags = "tag1, tag2";

_agile.create_contact(contact, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
####2.2 Get Contact

Contact can be searched based on email address.

- Parameters : contact email, callback

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
####2.3 Delete Contact

Deletes a contact based on email.

- Parameters : contact email, callback

```javascript
_agile.delete_contact("contact@test.com", {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```

####2.4 Update Contact

Updates the contact with given JSON data.

- Parameters : contact data, callback

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
####2.5 Set Contact Property

To add new or update existing contact property (*first_name*, *last_name*, *title*, *company*, *website*, *phone*, *address*) or any CUSTOM contact property.

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

- Parameters : property, callback

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
####2.6 Get Contact Property

To get contact property value based on name of the property, and email.

- Parameters : property name (string), callback

```javascript
_agile.get_property("title", {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```

####2.7 Remove Contact Property

To remove contact property based on name of the property and email set earlier using ```_agile.set_email```.

- Parameters : property name (string), callback

```javascript
_agile.remove_property("title", {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
###3.Tags

####3.1 Add Tags

Adds tags to the contact based on the email set earlier using ```_agile.set_email```. 

- Parameters : tags, callback

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
####3.2 Remove Tags

Removes tags from the contact based on email address set using ```_agile.set_email``` before.

- Parameters : tags, callback

```javascript
_agile.remove_tag('tag3, tag4, tag5', {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
####3.3 Get Tags

Gets the tags associated with the contact, based on the email address, set earlier using ```_agile.set_email```.

- Parameters : callback 

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
###4.Score
####4.1 Add Score

Add score to contact based on email address set, using ```_agile.set_email``` before making this API call.

- Parameters : score, callback

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
####4.2 Subtract Score

Subtract score of a contact based on email address set, using ```_agile.set_email``` before.

- Parameters : score, callback

```javascript
_agile.substract_score(5, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
####4.3 Get Score

Get score associated with contact set earlier, using ```_agile.set_email```.

- Parameters : callback

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
###5.Task
####5.1 Add Task

Add task to contact set earlier, using ```_agile.set_email```.

Task object has the properties **type** with values as *CALL* | *EMAIL* | *FOLLOW_UP* | *MEETING* | *MILESTONE* | *SEND* | *TWEET*, **priority_type** with values as *HIGH* | *NORMAL* | *LOW*, **subject** the subject of task and **due** which is the due date for the task.

- Parameters : task, callback

```javascript
var task = {};
task.type = "MEETING";
task.priority_type = "HIGH";
task.subject = "Sample Task";
task.due = "1376047332";

_agile.add_task(task, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
####5.2 Get Tasks

Get the tasks data associated with contact, based on email set in cookie, using ```_agile.set_email``` before calling this API method.

- Parameters : callback

```javascript
_agile.get_tasks({
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
###6.Note
####6.1 Add Note

Creates a new note. Note object has the properties **subject** the subject of the note, and **description** note description. Requires ```_agile.set_email``` before to set contact.

- Parameters : note, callback

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
####6.2 Get Notes

Get all the notes data associated with the contact set, using the ```_agile.set_email``` before calling this API method.

- Parameters : callback

```javascript
_agile.get_notes({
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
###7.Deal
####7.1 Add Deal

Add deal to contact. You need to set contact email using ```_agile.set_email``` before calling the API method. Deal object has the properties **name** name of the deal, **description** deal description, **expected_value**, **milestone**, **probability** and **close_date**.

- Parameters : deal, callback

```javascript
var deal = {};
deal.name = "Test Deal";
deal.description = "This is a test deal";
deal.expected_value = "10000";
deal.milestone = "won";
deal.probability = "95";
deal.close_date = "1376047332";

_agile.add_deal(deal, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
####7.2 Get Deals

Gets all deals data related to contact, based on email set using ```_agile.set_email``` before.

- Parameters : callback

```javascript
_agile.get_deals({
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
###8.Create Company

Add company as contact. Company object has the properties **name**, **phone**, **url**, **address**.

- Parameters : company, callback

```javascript
var company = {};
company.name = "abc inc";
company.phone = "+1-541-754-3010";
company.url = "http://www.abc-inc.com";
company.address = "{\"city\":\"new delhi\",\"state\":\"delhi\",\"country\":\"india\"}";

_agile.create_company(company, {
    success: function (data) {
        console.log("success");
    },
    error: function (data) {
        console.log("error");
    }
});
```
##More

You may refer to [create_contact.html](https://github.com/agilecrm/javascript-api/blob/master/create_contact.html) and [all.html](https://github.com/agilecrm/javascript-api/blob/master/all.html) for example implementation of multiple API methods simultaneously. 
