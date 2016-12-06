# Angular Maps
## Learning Objectives
- Create a map using angular using `ng-map`
- Add markers to a map
- Add Layers to a map

## Framing
There are lots of tools out there to build maps. We'll just be talking about one today, the google maps API. Specifically how to implement this tool using angular. There's also a couple different ways to implement this API with angular. Today we'll be using `ng-map`.

> For the purposes of this class, we will not need a Google Maps API key, but you may want to add one. Get an API key and add it as a parameter to the url when you link the script in the head. Look [here](https://developers.google.com/maps/documentation/javascript/get-api-key) to get a key.

## Setup
Turns out, Setting up an angular map is super easy. Let's create two files we'll need to get this working: `index.html` and `script.js`

In `index.html`:

```html
<!DOCTYPE html>
<html data-ng-app='app'>
<head>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/angular.js/1.5.9/angular.js"></script>
  <script src="/bower_components/ngmap/build/scripts/ng-map.min.js"></script>
  <script src="http://maps.google.com/maps/api/js"></script>
  <script src="script.js"></script>
</head>
<body>

</body>
</html>
```

> We're including a couple of things here. We have the angular dependency that we all know and love and of course our `script.js`. The two other dependencies are for google maps and its integration with angular.

The google maps API is given to us through a standard CDN. However, the `ng-map` dependency is installed using a tool called bower. Bower is a front-end dependency manager. To download it run:

```bash
$ npm install bower -g
```

Then we need to grab the `ng-map` dependency using bower:

```bash
$ bower install ngmap
```

> This will create a folder called `bower_components` and add the `ngmap` dependency to it.

## Our first angular map

Thanks to `ng-map`, building our first angular map is quite easy! Let's start by adding the module we defined in our `index.html`
to our `script.js`:

```js
let app = angular.module("app", ["ngMap"])
```

And... that's all folks. That is all the setup you need in your script file to get a basic map running. Let's add some content to our `index.html` so we can actually see a map:

```html
<ng-map center="[38.9072, -77.0369]" zoom="10"></ng-map>
```

> In this example there's a couple attributes being used, `center` and `zoom`. There are a lot more, here's a [blog post](http://allenhwkim.tumblr.com/post/70986888283/google-map-as-the-simplest-way) from the creator of the dependency that highlights them.

## Markers
Whats a map without markers? Here's how to add marker. In our `index.html`:

```html
<ng-map center="[38.9072, -77.0369]" zoom="10">
  <marker position="[38.9072, -77.0369]"></div>
</ng-map>
```

> Note there are ALOT more map directives that come out of the box from `ng-map`. This is just a couple. Checkout them out [here](https://ngmap.github.io/)

## Moar Functionality
Let's say we wanted more programmatic access to our maps. We currently only have the ability to add additional attributes and directives.

> These next sections of code, are more implementations of the Google Maps API in javascript rather than explicit angular code.

We're going add a controller to our application so that we can do that. In `script.js`:

```js
app.controller("map", ["NgMap", Callback])

function CallBack(NgMap){
  NgMap.getMap().then((map) => {
    let marker = new google.maps.Marker({
      position: {
        lat: 38.9072,
        lng: -77.0369
      },
      map: map,
      title: "Some Place in DC"
    })
  })
}
```

> You'll notice we've injected `NgMap` as a dependency to our controller, and as a result have access to it as an argument in the callback.

> We use the `NgMap` dependency to grab the map calling `.getMap`. `.getMap` is a function that grabs the map and then returns that map as data in the callback. Once we're in the promise, we add a marker to the map.

Great, we've added a marker using javascript now! We can get rid of the existing `marker` directive now too!

## Event Listeners on a Marker
Let's say we want something to happen when we click on the marker. In `script.js`:

```js
function CallBack(NgMap){
  NgMap.getMap().then((map) => {
    let marker = new google.maps.Marker({
      position: {
        lat: 38.9072,
        lng: -77.0369
      },
      map: map,
      title: "Some Place in DC"
    })
  })
  google.maps.event.addListener(marker, 'click', () => {
    console.log("marker was clicked!")
  })
}
```

## Info Windows

We can create info windows on our google maps too!

```js
function CallBack(NgMap){
  NgMap.getMap().then((map) => {
    let marker = new google.maps.Marker({
      position: {
        lat: 38.9072,
        lng: -77.0369
      },
      map: map,
      title: "Some Place in DC"
    })
  })
  google.maps.event.addListener(marker, 'click', () => {
    infoWindow = new google.maps.InfoWindow({content: "This is DC!"})
    infoWindow.open(map, marker)
  })
}
```

## Layers

Google maps comes with tons of layers, right out of the box! Here's a cool one. Add this to your `script.js`:

```js
function CallBack(NgMap){
  NgMap.getMap().then((map) => {
    let transitLayer = new google.maps.TransitLayer();
    transitLayer.setMap(map)
  })
}
```

Really, we're just scratching the surface of the google maps API. There are tons of things you can do and `ng-map` abstracts a lot of that functionality into directives.

## Useful Documentation
[Google Maps API Docs](https://developers.google.com/maps/documentation/)
[Angularjs Google Maps Docs](https://ngmap.github.io/)
