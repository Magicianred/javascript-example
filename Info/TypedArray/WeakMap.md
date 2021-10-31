# WeakMap

It contains only object.

## Create new

```javascript
const myMap = new WeakMap();
```

## Add elements

```javascript
const myKey = { name: 'John' };
myMap.add(myKey);
```

## Add elements with value

```javascript
const myKey = { name: 'John' };
myMap.set(myKey, myValue);
```

## Get element

```javascript
const myValue = myMap.get(myKey);
```

## Check if element exists

```javascript
myMap.has(myKey);
```

## Delete an element

```javascript
mySet.delete(myKey);
```
