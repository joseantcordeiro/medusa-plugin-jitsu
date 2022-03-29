# medusa-plugin-jitsu
Medusa commerce Jitsu data ingestion

# Jitsu

Modern e-commerce businesses have to integrate with a wide spectrum of tools from marketing and personalization to analytics and business intelligence. Integrations to these tools quickly become hard to maintain and new integrations become overly complex to implement putting a stretch on an e-commerce organization's resources.

The CDP (The Open Source Segment Alternative) [Jitsu](https://jitsu.com/) solves this problem by allowing users to instantly integrate with +100 tools through a single unified API.

Jitsu is based official plugin `medusa-plugin-segment` that instantly gives you access to all Segment integrations and comes preconfigured with powerful server-side tracking

## Why Jitsu?

Jitsu is a powerful Open Source Customer Data Platform that allows users to collect, transform, send and archive their customer data.

Jitsu allows users to manage different tracking and marketing tools using one API and interface, making it very simple to try out and integrate with different services in your e-commerce stack.

Common integration use cases that can be implemented with Jitsu include:

- Facebook
- Amplitude
- Google Analytics
- HubSpot
- dbt Cloud
- Mixpannel


## Adding Jitsu to your Medusa store

> Note: you should create a [Project in Jitsu](https://jitsu.com/docs/sending-data/api) in order to obtain the write key that will be provided in the plugin options.
Plugins in Medusa's ecosystem come as separate npm packages, that can be installed from the npm registry.

```bash
yarn add medusa-plugin-jitsu
```

After installation open `medusa-config.js` to configure the Jitsu plugin, by adding it to your project's plugin array and providing the options required by the plugin, namely the write key obtained from the Segment dashboard.

```jsx
{
    resolve: `medusa-plugin-jitsu`,
    options: {
      api_key: JITSU_API_KEY,
    }
}
```

After the plugin has been configured you will get instant access to +100 services through the Segment dashboard. This allows you to try out new services for your e-commerce stack without having to make heavy integration investments.

## Default tracking

`medusa-plugin-jitsu` comes with prebuilt tracking for common flows for Orders, Returns, Swaps, and Claims. Where applicable the events follow the [Segment Ecommerce Spec](https://segment.com/docs/connections/spec/ecommerce/v2/).

Below is a list of some of the events that are tracked by default:

- Orders
  - Order Completed
  - Order Shipped
  - Order Refunded ← Without returned products
  - Order Cancelled
- Returns
  - Order Refunded ← With returned products
- Swaps
  - Swap Created
  - Swap Confirmed
  - Swap Shipped
- Claims
  - Item Claimed

The default events serve as a good foundation for e-commerce tracking, allowing you to answer questions regarding product performance, return ratios, claim statistics, and more.

In many cases you will want to track other events that are specific to your store - this is also possible through the Jitsu plugin, as the plugin registers the `jitsuService` in your Medusa project.

## Tracking custom events

Building from the custom functionality that can be guided by [the tutorial](https://docs.medusajs.com/tutorial/adding-custom-functionality) in Medusa docs, imagine that you want to track all welcome opt-ins.

The `jitsuService` exposes a `track` method that wraps [Jitsu's Track Spec](https://jitsu.com/docs/sending-data/node-js), allowing you to send events to the Jiysu from anywhere in your Medusa project.

For example, to add tracking of the opt-ins in the `POST /welcome/:cart_id` endpoint, you could add the following code:

```jsx
const jitsuService = req.scope.resolve("jitsuService")
segmentService.track("Welcome Opt-In Registered", {
  event: "Welcome Opt-In Registered",
  properties: {
    cart_id,
    optin,
  },
})
```
