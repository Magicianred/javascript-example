# Javascript

## Interfaccia dell'iterable Asincrona

```javascript
const AsyncIterable = {
	[Symbol.asyncIterator]() {
		return AsyncIterator;
	}
}
```

## Interfaccia dell'iteratore

Solo il metodo *next* è obbligatorio

```javascript
const AsyncIterator = {
	next() {
		return Promise.resolve(IteratorResult); // da il valore successivo
	},
	return() {
		return Promise.resolve(IteratorResult); // effettua operazioni all'uscita del ciclo, esempio cleanup
	},
	throw(e) {
		throw Promise.reject(e); // ritorna un errore
	}
}
```

## Interfaccia IteratorResult

__Per le iterazioni asincrone il value non deve mai essere una Promise__

```javascript
const IteratorResult = {
	value: any,
	done: boolean,
}
```

## Esempio di uso for...of in un array

il loop for...of utilizza di base l'iterator

```javascript
(async function forOfAsync() {
	const source = [
		new Promise(res => setTimeout(res, 1000, 1)),
		new Promise(res => setTimeout(res, 2000, 2)),
		new Promise(res => setTimeout(res, 3000, 3)),
	];
	
	for await (const v of source) {
		console.log(v);
	}
})();
```

[TO COMPLETE]

