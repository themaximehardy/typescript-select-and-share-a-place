# Select & Share a Place App

### Getting User Input

```ts
// app.ts
const form = document.querySelector('form')!;
const addressInput = document.getElementById('address')! as HTMLInputElement;

function searchAddressHandler(event: Event) {
  event.preventDefault();
  const enteredAddress = addressInput.value;

  // send this to Google's API!
}

form.addEventListener('submit', searchAddressHandler);
```

### Using Axios to Fetch Coordinates for an Entered Address

```ts
// app.ts
import axios from 'axios';

const form = document.querySelector('form')!;
const addressInput = document.getElementById('address')! as HTMLInputElement;

const GOOGLE_API_KEY = process.env.GOOGLE_API_KEY || '';

type GoogleGeocodingResponse = {
  results: { geometry: { location: { lat: number; lng: number } } }[];
  status: 'OK' | 'ZERO_RESULTS';
};

function searchAddressHandler(event: Event) {
  event.preventDefault();
  const enteredAddress = addressInput.value;

  axios
    .get<GoogleGeocodingResponse>(
      `https://maps.googleapis.com/maps/api/geocode/json?address=${encodeURI(enteredAddress)}&key=${GOOGLE_API_KEY}`,
    )
    .then((res) => {
      if (res.data.status !== 'OK') {
        throw new Error('Could not fetch location!');
      }
      const coordinates = res.data.results[0].geometry.location;
      console.log(coordinates);
    })
    .catch((err) => {
      alert(err.message);
    });
}

form.addEventListener('submit', searchAddressHandler);
```

### Rendering a Map with Google Maps

```ts
import axios from 'axios';

const form = document.querySelector('form')!;
const addressInput = document.getElementById('address')! as HTMLInputElement;

const GOOGLE_API_KEY = process.env.GOOGLE_API_KEY || '';

type GoogleGeocodingResponse = {
  results: { geometry: { location: { lat: number; lng: number } } }[];
  status: 'OK' | 'ZERO_RESULTS';
};

// INJECT GOOGLE MAPS SCRIPT
const head = document.getElementsByTagName('head')[0];
const script = document.createElement('script');
script.defer = true;
script.src = `https://maps.googleapis.com/maps/api/js?key=${GOOGLE_API_KEY}`;
head.appendChild(script);

function searchAddressHandler(event: Event) {
  event.preventDefault();
  const enteredAddress = addressInput.value;

  axios
    .get<GoogleGeocodingResponse>(
      `https://maps.googleapis.com/maps/api/geocode/json?address=${encodeURI(enteredAddress)}&key=${GOOGLE_API_KEY}`,
    )
    .then((res) => {
      if (res.data.status !== 'OK') {
        throw new Error('Could not fetch location!');
      }
      const coordinates = res.data.results[0].geometry.location;

      const map = new google.maps.Map(document.getElementById('map') as HTMLElement, {
        center: coordinates,
        zoom: 16,
      });
      new google.maps.Marker({ position: coordinates, map: map });
    })
    .catch((err) => {
      alert(err.message);
    });
}

form.addEventListener('submit', searchAddressHandler);
```
