JavaScript API
==============
JavaScript Connector for Agile

You need to have an agile account to use the API.

Create an account at https://www.agilecrm.com

# Requirements
### Setting API & Analytics
- You may access the analytics code from ***Admin Settings -> API & Analytics -> Analytics Code***
- Copy the 6 lines of script and paste in it in your webpage's HTML just before the ```</BODY>``` tag, for which you need API methods and / or tracking.

![Finding Analytics Code] (https://github.com/agilecrm/javascript-api/blob/master/analytics_code_new.jpg)



#Basic Usage (Things to know)
###1. Tracking Contacts: 

```javascript
_agile.set_email('visitor@youwebsite.com')
```

Agile tracks contacts automatically from the emails clicks if you have tracking enabled (track and push) in your outbound emails in the campaigns or one-on-one emails. This step is not required unless you have a landing form or offer custom login for your application. Agile sets a cookie for a validity is for one full year. You do not have to do set_email for every visit. 

###2. Tracking page views

Agile tracks the page views for each session for each contact. The visitor is tracked as anonymous unless a set_email is done.  The correct step is to set_email and then track_page_view. Anonymous users are tracked and backtracked when a user email is added using set_email. All the old page views are attributed to the new email address. This is often the case where the user browses many sessions and then signups for your product or provides the email in the landing pages during product download.

###3. Email Override

If a new email address is provided, Agile treats them as a new user and the sessions are tracked for the new user. The old page views are still valid for the old email and can be accessed from Agile dashboard.

#API Usage

API can be used to sync data from your website/app to Agile CRM.

**Note: All API calls are Asynchronous. To execute multiple API calls simultaneously, you need to place the subsequent API call in the success callback of the previous call.**


###1.Tracking website visitors

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

####1.3 Tracking across multiple sites
It is possible to track a vistor on multiple subdomains of your site. i.e www.mysite.com and abc.mysite.com. 
The API call _agile.set_tracking_domain ('mysite.com') allows this. This call should be placed in between _agile.set_account() and  _agile.track_page_view() calls. 

When this is used, visitor email that is set using set_email on one of your sites, can be accessed using the get_email on the other sites (subdomains).

###2.Contact
####Format of the contact object is as follows

```javascript
{
    "id": 5834008696979456,
    "type": "PERSON",
    "created_time": 1411995091,
    "updated_time": 0,
    "viewed_time": 0,
    "star_value": 0,
    "lead_score": 0,
    "tags": [],
    "properties": [
        {
            "type": "SYSTEM",
            "name": "first_name",
            "subtype": null,
            "value": "Test"
        },
        {
            "type": "SYSTEM",
            "name": "last_name",
            "subtype": null,
            "value": "Contact"
        },
        {
            "type": "SYSTEM",
            "name": "email",
            "subtype": null,
            "value": "test@contact.com"
        },
        {
            "type": "SYSTEM",
            "name": "title",
            "subtype": null,
            "value": "QA"
        }
    ]
}
```

####2.1 Create Contact

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

####2.2 Get Contact

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

####2.3 Delete Contact

Deletes a contact (based on email already set using ```_agile.set_email```).

- Parameters : contact email, callback object (optional)

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
- Success data :

**data** parameter of the success function is

```javascript
{success: "Contact deleted successfully"}
```
- Error data :

**data** parameter of error callback is

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

####2.4 Update Contact

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

####2.5 Set Contact Property

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

####2.6 Get Contact Property

To get contact property value based on name of the property, and email set earlier using ```_agile.set_email```.

- Parameters : property name (string), callback object (optional)

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
- Success data : 

**data** parameter of the success callback is in the following format

```javascript
{value: "QA"}
```
Property value can be accessed as data.value

- Error data :

**data** parameter of the error callback is the error message object.

```javascript
{error: "Property not found for contact"}
{error: "Contact not found"}
{error:"Invalid API key"}
```

####2.7 Remove Contact Property

To remove contact property based on name of the property and email set earlier using ```_agile.set_email```.

- Parameters : property name (string), callback object (optional)

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
- Success data : 

**data** parameter of the success callback is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of the error callback is the error message object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

###3.Tags

####3.1 Add Tags

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

####3.2 Remove Tags

Removes tags from the contact (based on email already set using ```_agile.set_email```).

- Parameters : tags, callback object (optional)

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
- Success data : 

**data** parameter of the success callback method is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

####3.3 Get Tags

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
###4.Score
####4.1 Add Score

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

####4.2 Subtract Score

Subtract score of a contact (based on email already set using ```_agile.set_email```).

- Parameters : score, callback object (optional)

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

- Success data : 

**data** parameter of the success callback method is the updated <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-contact-object-is-as-follows' target='_blank'>contact object</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

####4.3 Get Score

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


###5.Task
####Format of the task object is as follows

```javascript
{
    "id": 5341427487735808,
    "type": "MEETING",
    "priority_type": "HIGH",
    "due": 1376047332,
    "created_time": 1412831338,
    "is_complete": false,
    "contacts": [
        {
            "id": 4692715627347968,
            "type": "PERSON",
            "star_value": 2,
            "lead_score": 0,
            "tags": [
                "tags"
            ],
            "properties": [
                {
                    "type": "SYSTEM",
                    "name": "first_name",
                    "subtype": null,
                    "value": "test"
                },
                {
                    "type": "SYSTEM",
                    "name": "last_name",
                    "subtype": null,
                    "value": "contact"
                },
                {
                    "type": "SYSTEM",
                    "name": "email",
                    "subtype": "",
                    "value": "test@contact.com"
                }
            ]
        }
    ],
    "subject": "Sample Task",
    "entity_type": "task",
    "notes": [],
    "note_description": null,
    "progress": 0,
    "status": "YET_TO_START",
    "taskOwner": {
        "id": 5066549580791808,
        "domain": null,
        "email": "sample@contact.com",
        "referer": {
            "referral_count": 0,
            "reference_by_domain": null
        }
    }
}
```

####5.1 Add Task

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

####5.2 Get Tasks

Get the tasks data associated with contact (based on email already set using ```_agile.set_email```).

- Parameters : callback object (optional)

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
- Success data : 

**data** parameter of the success callback is the array of <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-task-object-is-as-follows' target='_blank'>task objects</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

###6.Note
####Format of the note object is as follows

```javascript
{
    "id": 6185852417867776,
    "created_time": 1412832296,
    "subject": "Test Note",
    "description": "This is a test note",
    "contact_ids": [
        "4692715627347968"
    ],
    "owner_id": "5066549580791808",
    "entity_type": "note",
    "contacts": [
        {
            "id": 4692715627347968,
            "type": "PERSON",
            "star_value": 2,
            "lead_score": 0,
            "tags": [
                "tags"
            ],
            "properties": [
                {
                    "type": "SYSTEM",
                    "name": "first_name",
                    "subtype": null,
                    "value": "test"
                },
                {
                    "type": "SYSTEM",
                    "name": "last_name",
                    "subtype": null,
                    "value": "contact"
                },
                {
                    "type": "SYSTEM",
                    "name": "email",
                    "subtype": "",
                    "value": "test@contact.com"
                }
            ],
            "owner": {
                "id": 5066549580791808,
                "email": "sample@contact.com"
            }
        }
    ],
    "domainOwner": {
        "id": 5066549580791808,
        "domain": null,
        "email": "sample@contact.com"
    }
}
```

####6.1 Add Note

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

####6.2 Get Notes

Get all the notes data associated with the contact (based on email already set using ```_agile.set_email```).

- Parameters : callback object (optional)

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
- Success data : 

**data** parameter of the success callback is the array of <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-note-object-is-as-follows' target='_blank'>note objects</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```

###7.Deal
####Format of the deal object is as follows

```javascript
{
    "id": 5622902464446464,
    "name": "Test Deal",
    "contact_ids": [
        "4692715627347968"
    ],
    "description": "This is a test deal",
    "expected_value": 10000,
    "milestone": "won",
    "probability": 95,
    "close_date": 1376047332,
    "owner_id": "5066549580791808",
    "created_time": 1412833056,
    "track": null,
    "entity_type": "deal",
    "notes": [],
    "note_description": null,
    "pipeline": {
        "id": 4532186929692672,
        "milestones": "1,2,3",
        "name": "track"
    },
    "pipeline_id": 4532186929692672,
    "contacts": [
        {
            "id": 4692715627347968,
            "type": "PERSON",
            "star_value": 2,
            "lead_score": 0,
            "tags": [
                "tags"
            ],
            "properties": [
                {
                    "type": "SYSTEM",
                    "name": "first_name",
                    "subtype": null,
                    "value": "test"
                },
                {
                    "type": "SYSTEM",
                    "name": "last_name",
                    "subtype": null,
                    "value": "contact"
                },
                {
                    "type": "SYSTEM",
                    "name": "email",
                    "subtype": "",
                    "value": "test@contact.com"
                }
            ],
            "owner": {
                "id": 5066549580791808,
                "email": "sample@contact.com"
            }
        }
    ],
    "owner": {
        "id": 5066549580791808,
        "email": "sample@contact.com"
    }
}
```

####7.1 Add Deal

Add deal to contact (based on email already set using ```_agile.set_email```). Deal object has the properties **name** name of the deal, **description** deal description, **expected_value**, **milestone**, **probability** and **close_date**.

- Parameters : deal, callback object (optional)

```javascript
var deal = {};
deal.name = "Test Deal";
deal.description = "This is a test deal";
deal.expected_value = "10000";
deal.milestone = "won";
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

####7.2 Get Deals

Gets all deals data related to contact (based on email already set using ```_agile.set_email```)

- Parameters : callback object (optional)

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
- Success data : 

**data** parameter of the success callback is the array of <a href='https://github.com/agilecrm/javascript-api/blob/master/README.md#format-of-the-deal-object-is-as-follows' target='_blank'>deal objects</a>.

- Error data :

**data** parameter of error callback is the error object.

```javascript
{error: "Contact not found"}
{error:"Invalid API key"}
```
###8.Company
####Format of the company object is as follows

```javascript
{
    "id": 6748802371289088,
    "type": "COMPANY",
    "created_time": 1412833579,
    "updated_time": 0,
    "viewed_time": 0,
    "viewed": {
        "viewed_time": 0,
        "viewer_id": null
    },
    "star_value": 0,
    "lead_score": 0,
    "tags": [],
    "tagsWithTime": [],
    "properties": [
        {
            "type": "SYSTEM",
            "name": "name",
            "subtype": null,
            "value": "abc inc"
        },
        {
            "type": "SYSTEM",
            "name": "phone",
            "subtype": null,
            "value": "+1-541-754-3010"
        },
        {
            "type": "SYSTEM",
            "name": "url",
            "subtype": null,
            "value": "http://www.abc-inc.com"
        },
        {
            "type": "SYSTEM",
            "name": "address",
            "subtype": null,
            "value": "{\"city\":\"new delhi\",\"state\":\"delhi\",\"country\":\"india\"}"
        }
    ],
    "owner": {
        "id": 5066549580791808,
        "email": "sample@contact.com"
    }
}
```
####8.1 Create Company

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

##More

You may refer to [create_contact.html](https://github.com/agilecrm/javascript-api/blob/master/create_contact.html) and [all.html](https://github.com/agilecrm/javascript-api/blob/master/all.html) for example implementation of multiple API methods simultaneously. 
