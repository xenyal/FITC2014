FITC2014
========


###### About [FITC](http://fitc.ca/)
### Future. Innovation. Technology. Creativity.

FITC produces design and technology events worldwide which inspire, educate and challenge attendees. Since 2002, FITC has brought together like-minded professionals and students in Toronto, Amsterdam, Tokyo, San Francisco, Chicago, Seoul, New York, Los Angeles and many other cities. Staying fresh by covering relevant topics in interactive, technical, design and business related content, FITC Events provides the professional development and networking opportunities needed to keep attendees up to date on the rapidly changing industry they work in.


## Presentation 1: Material Design with AngularJS

#### Context: Page Speed
- Average worldwide roundtrip time to google is ~100ms
- Mobile RTT is 100 - 1000ms
- If cellular radio is idle, add 1000 - 2000ms

#### Concurrent Connection Limits
- Maximum connections to one server/proxy: Less than 10
- Faking multiple domains from one server is workaround for max connections but is bad practice 
- Causes performance issues esp for mobile

## Image Compression
Paradigm is moving from raster to vectors, that is - going from JPEG/PNG to SVG. In terms of software, moving from Photoshop to Illustrator.

#### JPEG
- JPEG over all other image types (GIF/PNG/Whatnot)
- JPEG colors have different data requirements: 
- Blue (400nm) - Red (700nm) -> We don't like red because it's less chunky data
- Greyscale compression is poor
- FileOptimizer for Windows, ImageOptim for MacOSX, PUNYpng on the web are lossless optimization software that reduces size between 15-25%

#### PNG
- FileOptimizer for Windows, Smush.It on the web. Other freeware are lossy which will result in color/quality distortions.

#### SVG
- Tiny file footprint but can be scaled to any size
- Most browser supports it and software exports it
- Compressor.io will compress it but is lossy

#### Web Fonts
- Pros: Small file sizes - Renders crisply at any size
- Cons: One color - Overkill?
- Scales to any size
- Check out [IcoMoon](https://icomoon.io) for free shit

#### Base64 Encoding
- Embedding graphics as part of your page is a tradeoff
- 20% bigger file size, but reduces server connections
- Makes code hella ugly
```HTML
<img alt="" src="Q873847983264W2FJUSWHGFHJSKJASDKJF">
```
#### JPEG + PNG + SVG = Magic
- Will save a fuckton of space

```xml
<svg>
	<def>
		<mask>
			<img>
		</mask>
	</def>
</svg>
```
#### Sprites
- Check out [**InstantSprite**](http://instantsprite.com)
- Connection savings, size spacings

## Loading Order
- CSS First, Javascript Last!!
- Javascripts will be in the head will stop further operations and slow your page rendering
- Something like Google Analytics script is exception to this rule

## Raw Data
- Choose JSON over XML
- JSON is smaller and doesn't require parsing
- XML is human readable but will slow down browser

## Javascript Hangups
#### - Variables are faster than objects properties and array items
#### - DOM Operations are slow, so minimize them!
#### - Searching the DOM is slow, and prone to change
#### - Change CSS classes and not styles

## Loading UI'
#### - Avoid spinners or users will kill themselves
#### - Transitions that distract the eye while data loads (Google card flip lol)
#### - Framing your content to distract them
#### - Lazy Image loading:
   As users scroll the page, have javascript load additional images.
Ensure that the feature is wrapped in a `<noscript>` tag
#### - Responsive Design Patterns
   WURFL.io will detect user device and browser version
#### - Using server side compression:
   Use GZIP on the server

## Presentation 2: Planning a Front-end JS App is hosted [**here**](http://www.codylindley.com/spotlight-front-end)

## Presentation 3: Enemy of the State

#### Server Side Rendering 
- Can be "Stateless", refreshing causes server side refresh
- Increase in functonality will gradually increase complexity
- Horrible for User experience but great for devs

#### DOM & Ajax Libs
- jQuery, MooTools, Prototype, etc.
- Manages UI state
- Renders UI on the server

#### Observer Frameworks
- Backbone, Angular, Ember, etc.
- Manages UI state, Data, and code
- Talks to server via JSON to get data
- Renders Javascript in response to UI state
- Stitch components through use of observers

#### One Way Flow Frameworks
- Flux pattern
- Framework that is widely used that demonstrates this is ReactJS
- Manages UI, data, and code
- Separates UI from data
- Everything is responsible for one thing:
	- Example: UI -> Action -> Callbacks -> Change events -> React Views (Full circle)

## Presentation 4: Javascript Promises
Is a method that prevents code structure where we have `n-th` level callback response-requests. Slides can be found [**here**](http://yto.io/xpromise).

#### Promises look like this:

```javascript
var promise = $http.get(url);
// add a handler somewhere else

promise.then(function(response) {
	// handle the response
});

promise.then(function(response) {
	// handle the same response again
});
```

#### Promises vs Events:
(For cases when promises aren't the answer)
- Promises: 
   - Things that happen ONCE
   - Same treatment for past and future calls
   - Easily matched with requests
- Events (Publish/Sub): 
   - Things that happen MANY TIMES
   - Only cares about future 
   - Detached from requests

#### Chained Promises are ugly:

```javascript
$http.get(url)
.then(function(resp){
	return response.data;	
});
.then(function(tasks) {
	return filterTasksAsynchronously(tasks);
});
.then(function(tasks) {
	return morestuff(tasks);
})
```

#### Here's how to make it better:

```javascript
var responsePromise = $http.get(url);

var tasksPromise = responsePromise.then(function(resp) {
	return response.data;
})

var filteredTasksPromise = tasksPromise.then(function(tasks) {
	return filterTasksAsynchronously(tasks);
});

var vmUpdatePromise = filteredTasksPromise.then(function(tasks) {
	$log.info(tasks);
	vm.tasks = tasks;
});

var errorHandlerPromise = vmUpdatePromise.then(null, function(err) {
	$log.error(error);
})
```

#### Axiom: `.then()` Returns a Promise. Always.

```javascript
var dataPromise = getDataAsync(query);
var xformedDataPromise = dataPromise
.then(function(results) {
	return transformData(results);
});

// trasnformedDataPromise will be a promise.
```

| When the callback...  | then `.then()` returns a promise that... |
| ------------- | ------------- |
| returns a regular value  | resolves to that value. |
| returns a promise  | resolves to the same value |
| throws an exception  | rejects with the exception |

#### Catching Rejections (The "ehn" way):

```javascript
.then(function(tasks) {
	$log.info(tasks); // If error occurs here, you're fucked LOL.
	vm.tasks = tasks;
}, function(error) {
	$log.error(error);
});
```

#### Catching Rejections (Preferred):

```javascript
$http.get(url)
	.then(function(resp) {
		return response.data;
	})
	.then(function(tasks) {
		return filterTasksAsynchronously(tasks);
	})
	.then(function(tasks) {
		$log.info(tasks);
		vm.tasks = tasks;
	})
	.then(null, function(err) {
		$log.error(error);
	});
```

#### Or pass the whole thing:

```javascript
return $http.get(url)
	// blah blah
	.then(function(tasks)) {
		$log.info(tasks);
		vm.tasks = tasks;
	}); // error handling will not be done in this case.
	    // Simplify the architecture and push issue down the controller.
```

#### Testing Promises: 

```javascript
it('should get tasks', function() {
	var tasks = getService('tasks');
	//Return a promise
	return tasks.getTasks()
		.then(function(tasks){
			expect(tasks.length).to.equal(1);
		}); // no ugly done callbacks
});
```
#### Making Promises(Modern):
I did not record the old school way.
Preferred way [**here**](github.com/rangle/angular-promisify).

```javascript
var getFooPromise = denodeify(getFooWithCallbacks); // denodeify is a library

return getFoodPromise()
	.then(function(result) {
		// code here;
	})
```

#### Do's and Don'ts:
- Promise chains are considered harmful
- Stay consistent:
   - A function that returns a promise should always return a promise and never throw.
      - Return `$q.reject(error)` instead of throwing.
      - Wrap non-promise return values in `$q.when()`.
- When in doubt, return a promise:
   - If unsure of synchronicity, assume async and return promise.
- Pass the Buck, But Don't Drop It: 
   - A function that receives a promise should return a promise, only handling errors that it is prepared to handle.
   - If you do not return a promise, then gg.

#### Avoid Optimistic Code:
- Avoid code that assumes that something has already happened.
   - Instead, ask for a promise, return a promise.
   - If you do write such code, name your functions to reflect this.

#### Neat Tricks with Promises:

```javascript
// Promise Caching
var tasksPromise;
function getTasks() {
	taskPromise = taskPromise || getTasksFromServer();
	return taskPromise;
}

// Prefetching
var tasksPromise = getTasksFromServer();
function getTasks() {
	return taskPromise;
}

// Postponing Requests
function get(path) {
	return user.waitForAuth();
	.then(function() {
		return $http.get(path);
	})
	.then(function(response) {
		return response.data;
	})
}; // Will wait until user finished authentication and then and only then load respData

// ES6 KoaStyle Generator
app.use(function* () {
	var data = yield getPromiseForData();
	// Proceed to use data
	console.log(data.items);
})
```

## Presentation 5: How to Test Front-End the Right Way
Slides: [**YearOfMoo**](yom.nu/fitc-spotlight-testing-slides)
Code: [**GitHub**](yom.nu/fitc-spotlight-testing-code)

#### Testing Example:

```javascript
// Function being tested
function evenNumbers(array) {
	var newNumb = [];
	for (i=0;i<array.length;i++) {
		var number = array [i];
		if (number % 2 == 0) {
			newNumb.push(Number)
		}
	}
	return newNumb;
}
```

## Presentation 6: Data Visualization
- Didn't learn much. Refer to sample files (Run Chrome with --disable-web-security from cmd to run)

## Presentation 7: Material Design with AngularJS 
Wao.. 10 second slides are op LOL h e l p.......




