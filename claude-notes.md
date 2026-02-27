# Ably Config & Notes for Claude

## API Key
74GMLQ.EAUWhQ:KsOte05eYk10UCBweQBwitSMO-mC-kAG9FH_9r2oxgc

## LiveObjects (v2.16+ API) — Quick Reference

CDN scripts:
```html
<script src="https://cdn.ably.com/lib/ably.min-2.js"></script>
<script src="https://cdn.ably.com/lib/liveobjects.umd.min-2.js"></script>
```

Plugin setup (the old `objects.umd.min-2.js` / `AblyObjectsPlugin` / `channel.objects.getRoot()` API is deprecated):
```js
const client = new Ably.Realtime({
  key: API_KEY,
  clientId: CLIENT_ID,
  plugins: { LiveObjects: AblyLiveObjectsPlugin },
});

const channel = client.channels.get('channel-name', {
  modes: ['PUBLISH', 'SUBSCRIBE', 'OBJECT_SUBSCRIBE', 'OBJECT_PUBLISH'],
});

// Get root PathObject (wraps root LiveMap)
const root = await channel.object.get();

// Read a value: root.get('key') returns PathObject, use .value() for primitive
const val = root.get('someKey')?.value();

// Write a value
await root.set('someKey', 'someValue');

// Subscribe to changes
root.subscribe(() => { /* re-read values */ });

// PathObject methods: get, set, remove, subscribe, batch, compact, compactJson,
//   increment, decrement, value, instance, entries, keys, values, size
```

Key gotchas:
- Must set channel modes including OBJECT_SUBSCRIBE and OBJECT_PUBLISH
- `channel.object.get()` (not `channel.objects.getRoot()`)
- `root.get('key')` returns a PathObject, call `.value()` for the raw value
- `LiveMap.create()` and `LiveCounter.create()` for inline object creation

## Demos Built
- seat-booking-demo.html: Theatre seat booking with LiveObjects (170 seats, real-time sync)
