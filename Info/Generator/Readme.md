# Generator

permette di creare funzioni che non hanno bisogno di venire completate, ma possono essere messe in "pausa" e può scambiare messaggi fra il chiamante e il ricevente

questo approccio permette di creare funzioni async scrivendole come sync

Non è possibile utilizzare la sintassi arrow per i generatori

la keyword *yield* permette di ritornare un valore, mette in pausa la funzione e si aspetta un messaggio per riprendere

Quando un generatore viene invocato, un iteratore viene restituito

Sintassi base:

```javascript
function* generator(...args) {
	yield;
}
```

## Esempio di generatore con funzione .next()

```javascript
function* generatorABC() {
	yield 'A';
	yield 'B';
	return 'C';
}

const iterator = generatorABC();

iterator.next(); // torna il primo yield come IteratorResult - { value: 'A', done: false }
iterator.next(); // torna il primo yield come IteratorResult - { value: 'B', done: false }
iterator.next(); // torna il primo yield come IteratorResult - { value: 'C', done: true }
iterator.next(); // torna il primo yield come IteratorResult - { value: undefined, done: true }
```

## Esempio di generatore con operatore spread

Da notare che l'operatore spread utilizza solo i valore che hanno done a false, quindi verrà scartato il valore associato a *return*

```javascript
function* generatorABC() {
	yield 'A';
	yield 'B';
	return 'C';
}

const iterator = generatorABC();

console.log([...iterator]) // ['A','B']
```

### Esempio di generatore che riceve dei parametri

```javascript
function* generatorCalculator(operator) {
	const firstOperand = yield;
	const secondOperand = yield;
	
	switch(operator) {
		case '+':
			yield firstOperand + secondOperand;
			return;
		case '-':
			yield firstOperand - secondOperand;
			return;
	}
}

const calculatorIterator = generatorCalculator('+');

calculatorIterator.next(); // instanzia il corpo del generatore (nessun yield tornato)
calculatorIterator.next(10); // setta il valore di *firstOperand*
console.log(calculatorIterator.next(5)); // setta il valore di *secondOperator* e ritorna il valore della somma 
calculatorIterator.next(); // arriva al return e termina
```

## Nested generator / iterable

Permette di inserire un generatore (o un iterable - ad esempio un array - in generale) all'interno di un altro generatore

```javascript
function* generatorABF() {
	yield 'A';
	yield 'B';
	const innerIterator = generatorCDE();
	yield* innerIterator;
	return 'C';
}

function* generatorCDE() {
	yield 'D';
	yield 'E';
	return 'F';
}

const iterator = generatorABC();

console.log([...iterator]) // ['A','B']
```


## Creazione di un iterable Custom attraverso un generatore

```javascript
const brands = {
	brand1: "Brand one",
	brand2: "Brand two",
	brand3: "Brand three",
	
	*[Symbol.iterator]() {
	
		const items = Object.values(this);
		for(const brand of items) {
			yield brand;
		}
	}
	
	// oppure semplifichiamo ancora
	// *[Symbol.iterator]() {
	// 	yield* Object.values(this);
	// }
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


## Creazione di un iterable Custom attraverso un generatore e l'uso di un filtro

```javascript
const brands = {
	brand1: "ABrand one",
	brand2: "BBrand two",
	brand3: "CBrand three",
	
	*[Symbol.iterator](predicate) {
	 	yield* Object.values(this).filter(predicate);
	}
};

const brandsIterable = brands[Symbol.iterator](name => name[0] === 'A');

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

## Usare un generatore per gestire le promise

Come usare un generatore per sostituire l'uso delle .then nelle promise

con .then
```javascript
fetch(endpoint)
	.then(response => response.json())
	.then(json => console.log(json));
```

con il generator
```javascript
function* getRequest(endpoint) {
	const response = yield fetch(endpoint);
	const json = yield response.json();
	console.log(json);
}
```