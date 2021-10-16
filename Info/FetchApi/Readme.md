# Fetch API

## AbortController()

per annullare la promise

```javascript
const controller = new AbortController();

setTimeout(() => controller.abort(), 1000);

try {
	fetch(url, {
		signal: controller.signal
	});
} catch(err) {
	if(err.name == 'AbortError') {
		console.log('Aborted!');
	} else {
		throw err;
	}
}

console.log(controller.signal.aborted); // false

controller.signal.addEventListener('abort', () => console.log('abort!'));

controller.abort(); // in console: abort!

console.log(controller.signal.aborted); // true
```

### Esempio di uso dell'abort controller su più fetch

```javascript
const urls = [...];

const controller = new AbortController();

const fetchJobs = urls.map(url => fetch(url, {
	signal: controller.signal
}));

const results = await Promise.all(fetchJobs);
```

[TO COMPLETE]