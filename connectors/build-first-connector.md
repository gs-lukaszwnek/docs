---
url: >-
  https://developer-portal.gainsight.com/docs/connectors/build-first-connector.md
description: >-
  A hands-on tutorial that walks you through creating a connector, storing
  credentials securely, and calling it from widget code
---

# Build Your First Connector

In this tutorial, you will create a connector that calls an external weather API, store credentials securely as a secret, test the connection, and call it from widget code. By the end, you will have a working connector that fetches live data.

## Before You Start

* You have access to Connectors in your platform (**Integrations** → **Developer Studio** → **Connectors**)
* You have an API key for a weather service — sign up at [weatherapi.com](https://www.weatherapi.com) for a free key
* Optional: a published widget to call the connector from (see [Your First Widget](/custom-widgets/v2/build-first-widget))

## Step 1: Store Your API Key as a Secret

Navigate to **Integrations** → **Developer Studio** → **Secrets and Variables**.
Click **Create Secret** and fill in:

* **Name**: `weather_api_key`
* **Value**: your WeatherAPI key
  Click **Save**.

You should see the secret appear in your secrets list. The value is hidden — the platform stores it encrypted and never exposes it to the browser.

## Step 2: Create the Connector

Navigate to **Integrations** → **Developer Studio** → **Connectors**. Click **Create Connector** and fill in:

* **Connection Name**: `Weather API`
* **Endpoint URL**: `https://api.weatherapi.com/v1/current.json`
* **HTTP Method**: `GET`

Click **Save**.

You should see a new connector in your connector list. The platform derives the permalink `weather-api` from the name — you will use this identifier when calling the connector from widget code.

## Step 3: Configure Authentication

Open the connector you just created. In the **Authentication** section, set **Authentication Type** to **API Key** and fill in:

* **Key**: `key`
* **Value**: {{ get\_secret('weather\_api\_key') }}
* **In**: `Query Parameter`

Click **Save**.

You should see the authentication section updated to show API Key with your secret reference. The platform resolves {{ get\_secret('weather\_api\_key') }} at request time — the actual key is never stored in the connector definition.

## Step 4: Test the Connection

Click **Test Connection** in the connector editor toolbar. In the test form, add a query parameter:

* **Key**: `q`
* **Value**: `London`

Click **Run Test**.

You should see a successful response with weather data for London — temperature, condition, and location details. If you see a `401 Unauthorized` error, open **Integrations** → **Developer Studio** → **Secrets and Variables**, verify the secret name matches exactly (names are case-sensitive), and re-run the test.

## Step 5: Call the Connector from Widget Code

Open your widget HTML file and add the following code (the SDK is loaded automatically by Customer Community — no script tag needed):

```html
<script>
(async () => {
  const sdk = new window.WidgetServiceSDK();
  try {
    const data = await sdk.connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: { q: "Warsaw" }
    });
    console.log("Weather data:", data);
  } catch (error) {
    console.error("Connector request failed:", error);
  }
})();
</script>
```

Publish the widget and open it in your browser with Developer Tools open (F12 or Cmd+Option+I). Go to the **Console** tab.

You should see the weather data object printed — city name, temperature, condition, and other fields returned by the API.

## Step 6: Display the Data

Update the widget HTML to show the weather data on screen. Replace the `console.log` call with DOM manipulation inside the shadow root:

```html
<div id="weather">Loading...</div>
<script>
(async () => {
  const sdk = new window.WidgetServiceSDK();
  try {
    const data = await sdk.connectors.execute({
      permalink: "weather-api",
      method: "GET",
      queryParams: { q: "Warsaw" }
    });
    // Find the widget host and query inside its shadow root
    var hosts = document.querySelectorAll(
      'gs-cc-registry-widget[data-widget-type*="weather"]'
    );
    hosts.forEach(function(host) {
      var el = host.shadowRoot && host.shadowRoot.getElementById("weather");
      if (el) {
        el.textContent = data.location.name + ": " + data.current.temp_c + "°C, " + data.current.condition.text;
      }
    });
  } catch (error) {
    console.error("Connector request failed:", error);
  }
})();
</script>
```

The widget code runs inside Shadow DOM, so `document.getElementById()` cannot reach elements inside the widget. Instead, find the widget host element and query through its `shadowRoot`. See [Rendering & DOM](/custom-widgets/v2/rendering-and-dom) for details on this pattern.

Publish and open the widget again.

You should see your widget display a line like `Warsaw: 12°C, Partly cloudy` — live weather data fetched through the connector.

## What You Built

You created a connector that proxies requests to an external API, stored an API key securely as a secret, configured API Key authentication using a secret reference, verified the setup with the built-in test tool, and called the connector from widget code using the SDK.

## What's Next

* [Configuration](/connectors/configuration) — all connector fields and URL rules
* [Authentication](/connectors/authentication) — OAuth Client Credentials and other auth types
* [Template Variables](/connectors/template-variables) — dynamic values with Jinja2
* [Testing & Debugging](/connectors/testing-and-debugging) — performance headers and troubleshooting
* [Repository Registry](/connectors/repository-registry) — define connectors in code alongside your widgets
* [Example `connectors_registry.json` in the template repository](https://github.com/gainsight-hub/widgets-repository-template/blob/main/connectors_registry.json) — working connector definitions with authentication and response transformation
