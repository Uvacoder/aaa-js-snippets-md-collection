# code-tips-javascript
Code tips for clean code in Javascript

## Use the same vocabulary for the same concept

```javascript
// BAD
getUserInfo();
getClientData();
getCustomerRecord();

// GOOD
getUser();
```

## Use pronounceable and meaningful variable names

```javascript
// BAD
const yyyymmdstr = moment().format("YYYY/MM/DD");

// GOOD
const currentDate = moment().format("YYYY/MM/DD");
```

## Avoid Mental Mapping

```javascript
// BAD
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  // Wait, what is `l` for again?
  dispatch(l);
});

// GOOD
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  dispatch(location);
});
```

## Few Function Arguments

```javascript
// BAD
function createMenu(title, body, buttonText, cancellable) {
  // ...
}
createMenu("Foo", "Bar", "Baz", true);

// GOOD
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}
createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

## Don’t Use Magic Numbers

```javascript
// BAD
setTimeout(blastOff, 86400000); // What is 86400000?

// GOOD
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;
setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

##  Use Explanatory Varibles

```javascript
// BAD
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);


// GOOD
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

## Don't add unneeded context

```javascript
// BAD
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

// GOOD
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};
```

## Functions should do one thing

```javascript
// BAD
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}

// GOOD
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}
function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

## Function names should say what they do

```javascript
// BAD
function addToDate(date, month) {
  // ...
}
const date = new Date();
// It's hard to tell from the function name what is added
addToDate(date, 1);

// GOOD
function addMonthToDate(month, date) {
  // ...
}
const date = new Date();
addMonthToDate(1, date);
```

## No flags function parameters
```javascript
// BAD
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}

// GOOD
function createFile(name) {
  fs.create(name);
}
function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

## Encapsulate Conditionals

```javascript
// BAD
if (fsm.state === "fetching" && isEmpty(listNode)){
	// ...
}

// GOOD
function shouldShowSpinner(fsm, listNode) {
	return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)){
	// ...
}
```

## Remove Dead Code
```javascript
// BAD
function oldRequestModule(url){
	// ...
}
function newRequestModule(url){
	// ...
}

// GOOD
function newRequestModule(url){
	// ...
}
```

## Avoid Negative Conditionals

```javascript
// BAD
function isDOMNodeNotPresent(node) {
  // ...
}
if (!isDOMNodeNotPresent(node)) {
  // ...
}

// GOOD
function isDOMNodePresent(node) {
  // ...
}
if (isDOMNodePresent(node)) {
  // ...
}
```

## Don't over optimize

```javascript
// BAD
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}

// GOOD
for (let i = 0; i < list.length; i++) {
  // ...
}
```

## Use getters and setters
```javascript
// GOOD
class Person {
    constructor(name) {
        this.setName(name); // name is private
    }
    getName() { // a "getter"
        return this._name;
    }
    setName(newName) { // a "setter"
				// ... validate before updating
        if (newName === '') {
            throw 'The name cannot be empty';
        }
        this._name= newName;
    }
}
```

## Prefer ES6 classes over ES5 functions

```javascript
// BAD
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }
  this.age = age;
};
Animal.prototype.move = function move() {};


// GOOD
class Animal {
  constructor(age) {
    this.age = age;
  }
  move() {/* ... */}
}
``` 
