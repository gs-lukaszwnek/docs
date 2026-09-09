---
url: https://developer-portal.gainsight.com/docs/sdk/widget-sdk/overview.md
description: >-
  JavaScript SDK for calling connectors from widgets — globally available as
  window.WidgetServiceSDK, distinct from the sdk parameter passed to init(sdk);
  see SDK Concepts for how they relate
---

# Widget SDK

The Widget SDK is a JavaScript library your widget code uses to call [connectors](/connectors/) — the secure backend proxies that connect widgets to external APIs. Calling an external API directly from the browser would expose your credentials, so the SDK exists to route each request through the platform instead, keeping those credentials on the backend.

The SDK is globally available as `window.WidgetServiceSDK` inside every widget hosted in Customer Community. Customer Community loads it automatically — no script tag is required — so your widget constructs an instance and calls a connector by its [permalink](/connectors/configuration):

```html
<script>
  (async () => {
    const sdk = new window.WidgetServiceSDK();

    const data = await sdk.connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: { q: "Warsaw" }
    });

    console.log(data);
  })();
</script>
```

This `window.WidgetServiceSDK` is a different object from the `sdk` parameter the platform passes to your widget's `init(sdk)` function — see [SDK Concepts](/sdk/concepts) for how the two relate, or the [Widget Runtime Reference](/custom-widgets/v2/sdk-api-reference) for that parameter's full API.

The constructor takes an optional configuration object for defaults such as request headers and a timeout — see [Widget SDK Methods and Constructors](methods-constructors#constructor) for the available options.

## Next Steps

* [Widget SDK Methods and Constructors](methods-constructors) — full method signatures and error handling
* [Examples](examples) — real-world usage patterns
* [Configuration](/connectors/configuration) — set up connectors to call with the SDK
