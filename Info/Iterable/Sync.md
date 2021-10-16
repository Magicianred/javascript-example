# Javascript

## Interfaccia dell'iterable

```javascript
const Iterable = {
	[Symbol.iterator]() {
		return Iterator;
	}
}
```

## Interfaccia dell'iteratore

Solo il metodo *next* è obbligatorio

```javascript
const Iterator = {
	next() {
		return IteratorResult; // da il valore successivo
	},
	return() {
		return IteratorResult; // effettua operazioni all'uscita del ciclo, esempio cleanup
	},
	throw(e) {
		throw e; // ritorna un errore
	}
}
```

## Interfaccia IteratorResult

```javascript
const IteratorResult = {
	value: any,
	done: boolean,
}
```

## Esempio di uso nell'array

gli array in javascript implementano l'interfaccia Iterable

```javascript
const array = [5 ,10 , 15];

const arrayIterator = array[Symbol.iterator]();

console.log(arrayIterator.next()); // { value: 5, done: false }
console.log(arrayIterator.next()); // { value: 10, done: false }
console.log(arrayIterator.next()); // { value: 15, done: false }
console.log(arrayIterator.next()); // { value: undefined, done: true }
```

## Esempio di uso for...of in un array

il loop for...of utilizza di base l'iterator

```javascript
const array = [5 ,10 , 15];

for(const v of array) {
	console.log(v);
}
```

## Esempio di uso del destruttore sugli array

il destruttore degli array utilizza di base l'iterator

```javascript
const array = [5 ,10 , 15];

const [a, b] = array;
console.log(a,b);
```

## Esempio di uso dello spread operator sugli array

lo spread operator degli array utilizza di base l'iterator

```javascript
const array = [5 ,10 , 15];

const clone = [...array];
console.log(clone);
```

## Creazione di un iterable Custom

bisogna implementare su un oggetto il metodo *iterable* ([Symbol.iterator])

```javascript
const brands = {
	brand1: "Brand one",
	brand2: "Brand two",
	brand3: "Brand three",
	
	[Symbol.iterator]() {
	
		const values = Object.values(this); // costruisce una array partendo dalle proprietà dell'oggetto
		let index = 0;
		
		const iterator = {
			next() {
				let done = index >= values.length;
				let value = values[index++];
				
				const iteratorResult = { value, done };
				return iteratorResult;
			},
			[Symbol.iterator]() {
				return this;
			}
		}
		
		return iterator;
	}
};

const brandsIterable = brands[Symbol.iterator]();

console.log([...brandsIterable]); // spread operator

for(const brand of brandsIterable) {
	console.log(brand);
}

function iterate(iterator, cb) {
	for(const v of iterator) {
		cb(v);
	}
}

iterate(brandsIterable, console.log);

```

