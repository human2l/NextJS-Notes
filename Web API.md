# Geolocation API

```js
if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      //handle success
    },(error) => {
      //handle error
    });
  } else {
    x.innerHTML = "Geolocation is not supported by this browser.";
  }
```

