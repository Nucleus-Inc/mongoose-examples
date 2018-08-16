# mongoose-examples

Mongoose examples

## Setup

``TODO``

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

```
  try {
      let query = await Order.find()
          .populate('user') // Simple populate
          .populate('product')
          .populate({
            path: 'product',
            populate: { path: 'category' }
          }) // Deep populate
          .lean()
      console.log(query)
    } catch (ex) {
      console.log(ex)
  }
```

### Aggregate

``TODO``

### Embedded Queries

### Create Embedded Document

```
  try {
      let query = await User.findByIdAndUpdate('5b7230980000000000000000', {
        $push:{
          'addresses' :{
                zipCode: '83844',
                addressLine1: '901 Paradise Creek St,',
                addressLine2: '',
                city: 'Moscow',
                state: 'ID'
                country: 'USA'
          }
        }     
      }, {
        new: true // Return updated document
      })
      .select({ 'addresses': { '$slice': -1 } }) //Return only created embedded doc
      .lean()
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

### Retrieve one Embedded Document

```
  try {
      const mongoose = require('mongoose')
      
      let query = await User.aggregate([
        {
          $match: {
            '_id': mongoose.Types.ObjectId('5b7230980000000000000000')
          }
        }, {
          $unwind: '$addresses'
        }, {
          $match: {
            'addresses._id': mongoose.Types.ObjectId('5b72f0d20000000000000000')
          }
        },
        {
          $group: {
            _id: null,
            addresses: {
              $push: '$addresses'
            }
          }
        },
        {
          $project: {
            _id: 0,
            addresses: '$addresses'
          }
        }
      ])
      console.log(query[0].addresses[0])
    } catch (ex) {
      console.log(ex)
    }
```

### Update Embedded Document

```
  try {
      let query = await User.findOneAndUpdate({
        _id: '5b7230980000000000000000',
        'addresses._id': '5b72f0d20000000000000000'
        }, {
          $set:{
            'addresses.$.zipCode': '83844',
            'addresses.$.addressLine1': '901 Paradise Creek St,',
            'addresses.$.addressLine2': '',
            'addresses.$.city': 'Moscow',
            'addresses.$.state': 'ID'
            'addresses.$.country': 'USA'
        }     
      }, {
        new: true // Return updated document
      })
      .lean()
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```


### Delete Embedded Document

```
  try {
      let query = await User.findOneAndUpdate({
        _id: '5b7230980000000000000000',
        'addresses._id': '5b72f0d20000000000000000'
        }, {
          $pull:{
            'addresses': {
                _id: '5b72f0d20000000000000000'
             }
          }     
      }, {
        new: true // Return updated document
      })
      .lean()
      console.log(query)
    } catch (ex) {
      console.log(ex)
    }
```

## Docs Migration

``TODO``
