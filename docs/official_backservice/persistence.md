#How to use data persistence service

Persistence service provide Create, Update, Read, Delete service of data. The service use mongodb as it's storage. 
When you subscribe persistence service, you need to config your app at [Manage Console](https://crud.service.oceanclouds.com/) .

## Create your entity schema
In manage console, you can create muti-entities for data storage. The schema should follow mongoose schema rules.

A entity schema is like

```
{
  name:    String,
  binary:  Buffer,
  living:  Boolean,
  updated: { type: Date, default: Date.now }
  age:     { type: Number, min: 18, max: 65 }
  mixed:   Schema.Types.Mixed,
  _someId: Schema.Types.ObjectId,
  array:      [],
  ofString:   [String],
  ofNumber:   [Number],
  ofDates:    [Date],
  ofBuffer:   [Buffer],
  ofBoolean:  [Boolean],
  ofMixed:    [Schema.Types.Mixed],
  ofObjectId: [Schema.Types.ObjectId],
  nested: {
    stuff: { type: String, lowercase: true, trim: true }
  }
}
```

The permitted SchemaTypes are
* String
* Number
* Date
* Buffer
* Boolean
* Mixed
* ObjectId
* Array

You can read more information [here](http://mongoosejs.com/docs/schematypes.html).

## Set your entity privilege

**GROUP**

Persistence service will identify app user to three groups
* anonymous, when app user not login.
* user, when app user login.
* owner, when this data is created by this user.

In these three group, you can setup their privilege about the entity, such as canCreate, canUpdate,  canDelete, canDelete.

If user canRead is set to NO, and owner canRead is set to YES, means only can read the users entity data, the api will auto add a filter to only show the user's data.

**SCRIPT for CALL**

There are privilege for these calls

|Name|support script|API doc|
|:-|:-|:-|
|canCreate|No||
|canUpdate|Yes||
|canDelete|Yes||
|canRead|Yes||
|canQuery|Yes||


Script can enable a filter before the actual operation such as update. For example, a update script like this

    {
      "criteria": {
        "status" : "DRAFT"
        "someUserId":"$currentUserId"
      }
    }
Will first query out the combine criteria by _id of the update id and the status condition. So the final criteria is like this

    {
      "$and": [
        {
          "status": "DRAFT",
          "someUserId":"<replace by UserID>"
        },
        {
          "_id": "<replace by entity id>"
        }
      ]
    }

So, the script can be set up by your safty stretegy. User only can operate or see what your filter script let him do. It can help complex app to deal with complex data model.


## Search your entity data

You can search your data from the manage console. The criteria rules must follow [Search Entity Data By Criteria](http://doc.oceanclouds.com/api/en/backservice/data_persistence.html).


## Use service by app

Call persistence API please see the API doc [here](http://doc.oceanclouds.com/api/en/backservice/data_persistence.html).



