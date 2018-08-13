# mongoose-examples

Mongoose examples

## Schemas

### Definition

```
const mongoose = require('mongoose')

module.exports = () => {
  const schema = mongoose.Schema({
      name: {
        type: String,
        required: true
      },
      email: {
        type: String,
        required: true,
        // Add unique index
        index: {
          unique: true
        }
      },
      age:{
        //Optional field
        type: Number
      },
      addresses: [{
        zipCode:{
          type: String,
          required: true
        },
        addressLine1:{
          type: String,
          required: true
        },
        addressLine2:{
          type: String
        },
        city:{
          type: String,
          required: true
        },
        state:{
          type: String,
          required: true
        },
        country:{
          type: String,
          required: true
        }
      }]
  }, {
    // Automatically adds timestamps to document (createdAt, updatedAt)
    timestamps: true
  })

  return mongoose.model('User', schema)
}
```

### Validation

``TODO``

## Queries

### Find All Documents

```
  try {
      let query = await User.find()
        .lean() // Lean query, remove default doc functions added by mongoose and improve query response time
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

### Find Document by Id

```
  try {
      let query = await User.findById('5b7230980000000000000000')
        .lean() // Lean query, remove default doc functions added by mongoose and improve query response time  
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

### Create Document

```
  try {
      let query = await User.create({
        name: 'Igor',
        email: 'user@email.com',
        age: 27
      })
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

### Update Document by Id

```
  try {
      let query = await User.findByIdAndUpdate('5b7230980000000000000000', {
        $set:{
          name: 'Igor Lopes',
          email: 'user2@email.com',
          age: 28
        }     
      }, {
        new: true // Return updated document
      })
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

### Populate

``TODO``

### Aggregate

``TODO``

### Embedded Queries

``TODO``

## Docs Migration

``TODO``
