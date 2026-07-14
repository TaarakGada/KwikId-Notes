# Browser Geolocation (Chromium)

## What is it?
- The **Geolocation API** lets web pages access the user's physical location
- Built into all modern browsers — no library needed
- Requires explicit **user permission**
- Uses GPS (mobile), WiFi triangulation, or IP address to determine location

## How Accurate Is It?
| Method | Accuracy |
|--------|----------|
| GPS (mobile) | ~3-5 meters |
| WiFi Triangulation | ~20-50 meters |
| IP Address | City-level only |

## Basic Usage
```javascript
// Check if geolocation is supported
if ('geolocation' in navigator) {
  navigator.geolocation.getCurrentPosition(
    // Success callback
    (position) => {
      const { latitude, longitude, accuracy } = position.coords;
      console.log(`Lat: ${latitude}, Lon: ${longitude}`);
      console.log(`Accuracy: ${accuracy} meters`);
    },
    // Error callback
    (error) => {
      switch(error.code) {
        case 1: console.log('Permission denied'); break;
        case 2: console.log('Position unavailable'); break;
        case 3: console.log('Timeout'); break;
      }
    },
    // Options
    {
      enableHighAccuracy: true,  // Use GPS if available
      timeout: 10000,            // Max wait time (ms)
      maximumAge: 0              // Don't use cached position
    }
  );
} else {
  console.log('Geolocation not supported');
}
```

## Watch Position (Live Tracking)
```javascript
// Continuously update position (e.g., for tracking)
const watchId = navigator.geolocation.watchPosition(
  (position) => {
    updateMapPosition(position.coords.latitude, position.coords.longitude);
  },
  (error) => console.error(error)
);

// Stop watching when done
navigator.geolocation.clearWatch(watchId);
```

## Position Object
```javascript
position.coords.latitude       // e.g., 28.6139
position.coords.longitude      // e.g., 77.2090
position.coords.accuracy       // meters of accuracy
position.coords.altitude       // meters above sea level (if available)
position.coords.speed          // m/s (if moving)
position.coords.heading        // degrees from north (if moving)
position.timestamp             // Unix timestamp of reading
```

## Using with Google Maps
```javascript
navigator.geolocation.getCurrentPosition((position) => {
  const { latitude: lat, longitude: lng } = position.coords;
  
  const map = new google.maps.Map(document.getElementById('map'), {
    center: { lat, lng },
    zoom: 15
  });
  
  new google.maps.Marker({ position: { lat, lng }, map });
});
```

## Chromium-Specific Notes
- **HTTPS required** — Geolocation is blocked on HTTP sites
- On Linux/Chromium, GPS accuracy may be limited (no system GPS daemon by default)
- Chromium uses **Google Location Services** for WiFi-based location
- Permissions are stored per origin — user can revoke at any time
- You can override location in **DevTools → Sensors** for testing

## Good to Know
- Always handle the **permission denied** case gracefully
- Don't request location before the user understands why you need it
- Consider using `maximumAge` to cache location and reduce battery usage
- For high-frequency tracking (e.g., delivery apps), use `watchPosition`
