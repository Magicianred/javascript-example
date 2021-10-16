# Promise

ogni promise può avere uno o più *observer* .then
ogni observer gestirà lo stesso ritorno/errore separatamente

```javascript
const prom = new Promise((resolve, reject) => {
	resolve(10);
	reject(Error());
}

prom.then((value, err) => {
	// primo then
});

prom.then((value, err) => {
	// secondo then
});
```

## Promise.resolve(value)

funzioni statica che ritorna una promise già risolta

### Il problema del ritorno di oggetti Thenable che sono meglio evitare, ossia Promise che ritornano dei *then*

```javascript
Promise.resolve({ then() {} })
.then(...);
```

## Promise.reject(reason)

funzioni statica che ritorna una promise già rigettata


## Promise.all([p1,p2,...])

Il metodo Promise.all riceve come parametro un *iterable* (spesso un array) di Promise che verranno risolte sequenzialmente, al primo errore si terminerà ritornando l'errore. Se tutte le Promise hanno successo sarà ritornato un array con il ritorno di ogni Promise es:. [res1, res2, res3, ... ]

il tempo di ritorno della funzione sarà quello della Promise più lenta

## Promise.allSettled([p1,p2,...])

Come la funzione Promise.all, ma ritornerà tutti i risultati riusciti o errori in un array di oggetti, tipo: [{value: 42, status:"fulfilled"},{reason:"errore", status:"rejected"}]

il tempo di ritorno della funzione sarà quello della Promise più lenta

## Promise.race([p1,p2,...])

come le funzioni Promise.all e Promise.allSettled, ma si differenzia perché ritornerà solo la più veloce. E' possibile così creare un sistema di "timeout" delle promise, del tipo:

```javascript
Promise.race([
	longOperation(),
	timeoutOperation() // impostare nella Promise un setTimeout dopo tot secondi
])
.then(...)
.catch(...);
```

## Promise .finally

usato per introdurre operazioni di pulizia sia nel caso di riuscita che di errore della promise, senza duplicare il codice

```javascript
Promise.race([
	longOperation(),
	timeoutOperation() // impostare nella Promise un setTimeout dopo tot secondi
])
.then(...)
.catch(...);
.finally(() => {
	// cleanup code
});
```

