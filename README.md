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

### Setting Up a Google API Key
