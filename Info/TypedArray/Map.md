# Map

It's a dictionary. The key is unique. If you set already key, you update your value.

## Create new

```javascript
const myMap = new Map();
```

## Add elements

```javascript
myMap.set(myKey, myValue);
```

## Get element

```javascript
const myValue = myMap.get(myKey);
```

## Get all keys

```javascript
const myKeys = myMap.keys();
for(key of myKeys) {
    console.log(key);
}
```

## Get all values

```javascript
const myValues = myMap.values();
for(value of myValues) {
    console.log(value);
}
```

## Get all keys with values

```javascript
const myEntries = myMap.entries();
for(entry of myEntries) {
    console.log('myKey: ' + entry[0]);
    console.log('myValue: '+ entry[1]);
}
```

## Check if element exists

```javascript
myMap.has(myVariable);
```
