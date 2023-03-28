# MongoDbHW

## Insert Data
```
db.scores.insertMany([
    {"name":"Ramesh","subject":"maths","marks":87},
    {"name":"Ramesh","subject":"english","marks":59},
    {"name":"Ramesh","subject":"science","marks":77},
    {"name":"Rav","subject":"maths","marks":62},
    {"name":"Rav","subject":"english","marks":83},
    {"name":"Rav","subject":"science","marks":71},
    {"name":"Alison","subject":"maths","marks":84},
    {"name":"Alison","subject":"english","marks":82},
    {"name":"Alison","subject":"science","marks":86},
    {"name":"Steve","subject":"maths","marks":81},
    {"name":"Steve","subject":"english","marks":89},
    {"name":"Steve","subject":"science","marks":77},
    {"name":"Jan","subject":"english","marks":0,"reason":"absent"}
])
```

Outputs:
```
{
  acknowledged: true,
  insertedIds: {
    '0': ObjectId("6422f8c39a49726c16e0e0f1"),
    '1': ObjectId("6422f8c39a49726c16e0e0f2"),
    '2': ObjectId("6422f8c39a49726c16e0e0f3"),
    '3': ObjectId("6422f8c39a49726c16e0e0f4"),
    '4': ObjectId("6422f8c39a49726c16e0e0f5"),
    '5': ObjectId("6422f8c39a49726c16e0e0f6"),
    '6': ObjectId("6422f8c39a49726c16e0e0f7"),
    '7': ObjectId("6422f8c39a49726c16e0e0f8"),
    '8': ObjectId("6422f8c39a49726c16e0e0f9"),
    '9': ObjectId("6422f8c39a49726c16e0e0fa"),
    '10': ObjectId("6422f8c39a49726c16e0e0fb"),
    '11': ObjectId("6422f8c39a49726c16e0e0fc"),
    '12': ObjectId("6422f8c39a49726c16e0e0fd")
  }
}
```

## Give MongoDB statements (with results) for the following queries

### Find the total marks for each student across all subjects.
```
db.scores.aggregate([
  {$group: {_id: '$name', marks: {$avg: '$marks'}}}
])
```
**Output:**

```
[
  { _id: 'Alison', marks: 84 },
  { _id: 'Rav', marks: 72 },
  { _id: 'Jan', marks: 0 },
  { _id: 'Ramesh', marks: 74.33333333333333 },
  { _id: 'Steve', marks: 82.33333333333333 }
]
```
### Find the maximum marks scored in each subject.
```
db.scores.aggregate([
  {$group: {_id: '$subject', marks: {$max: '$marks'}}},
])
```
**Output:**
```
[
    { _id: 'maths', marks: 87 },
    { _id: 'english', marks: 89 },
    { _id: 'science', marks: 86 }
]
```
### Find the minimum marks scored by each student.
```
db.scores.aggregate([
  {$group: {_id: '$name', marks: {$min: '$marks'}}},
])
```
**Output:**
```
[
  { _id: 'Rav', marks: 62 },
  { _id: 'Alison', marks: 82 },
  { _id: 'Steve', marks: 77 },
  { _id: 'Ramesh', marks: 59 },
  { _id: 'Jan', marks: 0 }
]
```

### Find the top two subjects based on average marks.
```
db.scores.aggregate([
  {$group: {_id: '$subject', marks: {$avg: '$marks'}}},
  {$sort: {marks: -1}},
  {$limit: 2}
])
```
**Output:**
```
[ { _id: 'maths', marks: 78.5 }, { _id: 'science', marks: 77.75 } ]
```