# MongoDB

## Commands
MongoDB Shell  
Also a JS shell - can execute JS code

```js
help

db
show dbs
show collections

// create/switch to specified db
use [db]

// insert many documents
db.[collection].insert([{ ... }]) // array of js objects

// returns all documents; returns a cursor
db.[collection].find()
// specific query; returns a cursor
db.[collection].find({ ... })
// specific query; returns 0 or 1 document
db.[collection].findOne({ ... })

// updates 0 or 1 document
db.[collection].updateOne({ <query> }, { <operator> : { <updates> } })
// updates many documents
db.[collection].updateMany({ <query> }, { <operator> : { <updates> } })
// completely replace a document except the id
db.[collection].replaceOne({ <query> }, { <operator> : { <updates> } })

// deletes 0 or 1 document
db.[collection].deleteOne({ <query> })
// deletes many documents
db.[collection].deleteMany({ <query> })

// sample query operators
{'nested.properties': value}
{age: {$gt: 7}}
```

## Query

### Array size
```js
{"$expr": {"$gt": [{"$size": "$product_details"}, 1]}}
```

### Type
```js
// string
{"created_at": {"$type": "string"}}

// not string
{"created_at": {"$not": {"$type": "string"}}}

// array elements are also array
{"active_product": {"$elemMatch": {"$type": "array"}}}
```

### RegEx
```js
{"email": {"$regex": /@mailinator.com/i}}
{"email": {"$regex": "@mailinator.com", "$options": "i"}}
```



## Index

### Single
```js
db.prepayment.createIndex(
    {"transaction_id": 1},
    {"name": "transaction_id_unique_1", "unique": true}
)

db.flows.createIndexes(
    [{"created_at": 1}, {"flow_id": 1}, {"recovery_type_code": 1}]
)
```

### Compound
```js
db.billing_lines.createIndex(
    {
        "product_id": 1,
        "current_cycle_payout_date": 1,
        "line_type": 1,
        "status": 1
    },
    {"name": "product_id_payroll_date_1"}
)
```



## Update

### Many
```js
db.employees.updateMany(
    {
        "employee_status": "GRANDFATHERED",
        "previous_employee_status": "GRANDFATHERED"
    },
    {"$set": {"previous_employee_status": "ACTIVE"}}
)
```



## Aggregate

### Expression
```js
db.employees.aggregate(
    [
        {
            "$match": {
                "$expr": {
                    "$eq": [
                        "$employee_status",
                        "$previous_employee_status"
                    ]
                }
            }
        },
        {
            "$project": {
                "employee_id": 1,
                "employee_status": 1,
                "previous_employee_status": 1
            }
        }
    ]
)
```

### Count
```js
db.employees.aggregate(
    [
        {
            "$match": {
                "employee_status": "GRANDFATHERED",
                "previous_employee_status": "GRANDFATHERED"
            }
        },
        {"$count": "count"}
    ]
)
```

### Group
```js
// group, count, sort count in descending, and show first (highest count)
db.lines.aggregate([
    {"$match": {"status": {"$ne": "CANCELLED"}}},
    {"$group": {"_id": {"product_id": "$product_id"}, "count": {"$sum": 1}}},
    {"$sort": {"count": -1}},
    {"$limit": 1}
])

// group, count, filter counts greater than 1, and count filtered
// used for counting duplicates
db.disbursement_transactions.aggregate([
    {"$group": {"_id": {"disbursement_id": "$disbursement_id"}, "count": {"$sum": 1}}},
    {"$match": {"count" : {"$gt": 1}}},
    {"$count": "duplicates"}
])
```



## Tools

### Dump and restore
```js
mongodump --uri="mongodb://localhost:27017" --db=recovery -o recovery_service
mongorestore --nsFrom='recovery.*' --nsTo='recoveryx.*' recovery_service

// specific collection
mongodump --uri "mongodb://localhost:27017" --db user_profile --collection user_event --out "./loan_eligibility"
mongorestore --uri "mongodb://localhost:27018" --db profile --collection user_event "./loan_eligibility/user_profile/user_event.bson"
```

## Rename
```js
// database
mongodump --archive --db=recovery | mongorestore --archive  --nsFrom='recovery.*' --nsTo='recoveryx.*'

// collection
db.payslip_fields.renameCollection("payslip_fieldsx")
```



## Convert
```js
// date to date string
db.files.updateMany(
    {actual_remittance_date: null},
    [{$set: {actual_remittance_date: {$dateToString: {format: "%Y-%m-%d", date: "$created_at"}}}}]
)
```



## Profiling
```js
use company
db.setProfilingLevel(2)
db.getProfilingStatus()
```

## Check connection status
```js
use admin
db.serverStatus().connections
```

## Replica Set
```bash
vi /usr/local/etc/mongod.conf
replication:
replSetName: rs0

mongosh
rs.initiate({_id: "rs0", members: [{_id: 0, host: "127.0.0.1:27017"}] })

env
MONGODB_URL=mongodb://localhost:27017/deductions_uat?replicaSet=rs0
```

[Reference](https://gist.github.com/davisford/bb37079900888c44d2bbcb2c52a5d6e8)



## References

[Setup](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-database#install-mongodb)  
[Compass](https://www.mongodb.com/try/download/compass)  
[Install multiple MongoDB instances](https://www.codexpedia.com/devops/installing-multiple-instances-of-mongodb/)  
