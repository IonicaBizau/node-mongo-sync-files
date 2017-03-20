
# mongof

 [![Support me on Patreon][badge_patreon]][patreon] [![Buy me a book][badge_amazon]][amazon] [![PayPal][badge_paypal_donate]][paypal-donations] [![Version](https://img.shields.io/npm/v/mongof.svg)](https://www.npmjs.com/package/mongof) [![Downloads](https://img.shields.io/npm/dt/mongof.svg)](https://www.npmjs.com/package/mongof)

> Sync MongoDB collections with JSON files.

## :cloud: Installation

```sh
$ npm i --save mongof
```


## :clipboard: Example



```js
// Dependencies
var Mongof = require("mongof")
  , Faker = require("faker")
  ;

/**
 * generateFakeDataArray
 * Generates fake data for inserting it in the MongoDB database.
 *
 * @name generateFakeDataArray
 * @function
 * @return {Array} An array with fake data.
 */
function generateFakeDataArray() {
    var docs = [];
    for (var i = 0; i < 30; ++i) {
        docs.push({
            name: Faker.name.findName()
          , email: Faker.internet.email()
          , age: Faker.helpers.randomNumber(90)
        });
    }
    return docs;
}

// Create database instance
var MyDatabase = new Mongof({
    uri: "mongodb://localhost:27017/test"
  , collections: [{
        inputFile: __dirname + "/docs-in.json"
      , outputFile: __dirname + "/docs-out.json"
      , collection: "myCol"
      , autoInit: true
    }]
}, function (err, collections) {

    // Handle error
    if (err) { throw err; }

    var MyAwesomeCollection = collections.myCol;

    // Run a Mongo request
    MyAwesomeCollection.find({}).toArray(function (err, docs) {

        // Handle error
        if (err) { throw err; }

        // Output
        console.log("Documents: ", docs);

        // Insert
        MyAwesomeCollection.insert(generateFakeDataArray(), function (err, docs) {

            // Handle error
            if (err) { throw err; }

            console.log("Successfully inserted a new document: ", docs);
            debugger
            console.log("Check out the content of the following file: ", MyAwesomeCollection._options.outputFile);

            // Close database
            MyAwesomeCollection.database.close();
        });
    });
});
```

## :memo: Documentation


### `Mongof(options, callback)`
Creates a new instance of Mongof.

#### Params
- **Object** `options`: An object containing the following properties:
 - `collections` (Array): An array of objects with the needed fields for initing the collections that should be inited (default: `[]`).
 - `ignore_sync_for` (Array): An array with Mongo collection method names for that sync should be diabled (default: `[]`).
 - `ignore_callback_for` (array): An array with Mongo collection method names for that callback should be diabled (default: `["find", "findOne"]`).
 - `uri` (String): The databse uri.
 - `db` (String): The databse name. If provided, the `uri` field will be overriden, with the `localhost:27017` Mongo databse uri.
- **Function** `callback`: The callback function. Called with `err`, `collections` and `data`. `collections` is an object like this:
  ```js
  {
     "collectionName": <collectionObject>
  }
  ```

`<collectionObject>` is an object containing all Mongo collection methods.

#### Return
- **EventEmitter** The instance of Mongof object.

### `addInCache(uri, dbObj, colName, colObj)`
Cache database and collection in the internal cache.

#### Params
- **String** `uri`: The Mongo database URI string.
- **Database** `dbObj`: The database object.
- **String** `colName`: The collection name (optional).
- **Collection** `colObj`: The collection object (optional).

#### Return
- **Mongof** The `Mongof` instance.

### `getDatabase(uri, callback)`
Returns (via callback) a database object from cache or fetched via Mongo functions.

#### Params
- **String** `uri`: The Mongo database URI string
- **Function** `callback`: The callback function

#### Return
- **Mongof** The `Mongof` instance.

### `getCollection(dbUri, collection, callback)`
Returns (via callback) the collection object from cache

#### Params
- **String** `dbUri`: The Mongo database URI string
- **String** `collection`: Collection name
- **Function** `callback`: The callback function

#### Return
- **Mongof** The `Mongof` instance.

### `initCollection(options, callback)`
Inits the collection and returns the collection instance.

#### Params
- **Object** `options`: An object containing the following properties:
 - `inputFile` (String): The path to the input file.
 - `outputFile` (String): The path to the output file.
 - `collection` (String): The collection that should be synced.
 - `outFields` (String): An object with fields that should be exported/ignored on stringify (default: `{_id: 0}`).
 - `autoInit` (Boolean): If `true`, the collection will be inited with input data.
- **Function** `callback`: The callback function

#### Return
- **EventEmitter** The collection object.



## :yum: How to contribute
Have an idea? Found a bug? See [how to contribute][contributing].


## :sparkling_heart: Support my projects

I open-source almost everything I can, and I try to reply everyone needing help using these projects. Obviously,
this takes time. You can integrate and use these projects in your applications *for free*! You can even change the source code and redistribute (even resell it).

However, if you get some profit from this or just want to encourage me to continue creating stuff, there are few ways you can do it:

 - Starring and sharing the projects you like :rocket:
 - [![PayPal][badge_paypal]][paypal-donations]—You can make one-time donations via PayPal. I'll probably buy a ~~coffee~~ tea. :tea:
 - [![Support me on Patreon][badge_patreon]][patreon]—Set up a recurring monthly donation and you will get interesting news about what I'm doing (things that I don't share with everyone).
 - **Bitcoin**—You can send me bitcoins at this address (or scanning the code below): `1P9BRsmazNQcuyTxEqveUsnf5CERdq35V6`

    ![](https://i.imgur.com/z6OQI95.png)

Thanks! :heart:



## :scroll: License

[MIT][license] © [Ionică Bizău][website]

[badge_patreon]: http://ionicabizau.github.io/badges/patreon.svg
[badge_amazon]: http://ionicabizau.github.io/badges/amazon.svg
[badge_paypal]: http://ionicabizau.github.io/badges/paypal.svg
[badge_paypal_donate]: http://ionicabizau.github.io/badges/paypal_donate.svg
[patreon]: https://www.patreon.com/ionicabizau
[amazon]: http://amzn.eu/hRo9sIZ
[paypal-donations]: https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=RVXDDLKKLQRJW
[donate-now]: http://i.imgur.com/6cMbHOC.png

[license]: http://showalicense.com/?fullname=Ionic%C4%83%20Biz%C4%83u%20%3Cbizauionica%40gmail.com%3E%20(https%3A%2F%2Fionicabizau.net)&year=2014#license-mit
[website]: https://ionicabizau.net
[contributing]: /CONTRIBUTING.md
[docs]: /DOCUMENTATION.md
