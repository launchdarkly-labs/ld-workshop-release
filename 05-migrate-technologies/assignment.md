---
slug: migrate-technologies
id: vuvyprvoibhk
type: challenge
title: Migrating Technologies with LaunchDarkly
teaser: Reduce your migration risks using feature flags
notes:
- type: text
  contents: Risky migrations are a thing of the past. Let LaunchDarkly help you safely
    and efficiently move from your legacy technology to your moderized workload.
tabs:
- id: zrwsyzfuxcim
  title: LaunchDarkly
  type: browser
  hostname: launchdarkly
- id: cibzecc5t47s
  title: Toggle Outfitters
  type: service
  hostname: workstation
  port: 3000
- id: ehuou6wk4z79
  title: Code Editor
  type: service
  hostname: workstation
  port: 8080
difficulty: basic
timelimit: 600
lab_config:
  default_layout_sidebar_size: 0
---

# Lab 5: Migrating Technologies with LaunchDarkly

In our previous challenge, we implemented a shopping cart for our frontend. However, we discovered an error when we tested it. As it turns out, we need to migrate our backend billing component to use the new billing system. Let's update our code so we can fix the error.

## Create the API Flag

Let's create a new flag to control the migration to the Stripe API.

> **Remember:** You may see either a modal dialog or a full page UI when creating flags. Follow the instructions below based on which UI you see.

---

### Instructions

<details>
<summary><strong>Click for Modal Dialog Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Migrate to Stripe API
```
4. In the bottom left corner, click the **No template** button and change it to **Release**. This template will allow you to set variation names after creating the flag.
5. Click **Create flag** in the lower right-hand side of the modal.
6. After the flag is created, select the **Variations** tab at the top of the page.
7. Update the variation names:
   - First variation **Name**: 
```js
Stripe Checkout Enabled
```
   - Second variation **Name**:
```js
Stripe Checkout Disabled
```

</details>
&nbsp;
<details>
<summary><strong>Click for Full Page Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Migrate to Stripe API
```
4. Under **Configuration**, select **Release**
5. Under **Variations**:
   a. First **Name**:
```js
Stripe Checkout Enabled
```
   b. Second **Name**:
```js
Stripe Checkout Disabled
```
6. Click **Create flag** in the lower right-hand side of the screen.

</details>
&nbsp;

As we've seen in previous challenges, our new code will be disabled whether the flag is on or off. In order to allow our Developers to test the new feature, let's add targeting to our new flag.

---

### Instructions

1. Click **+ Add rule** and choose **Target segments**
1. From the **Segments** dropdown, select *Developers*
1. From the **Rollout** dropdown, select *Stripe Checkout Enabled*
1. Toggle the flag to **On** in the upper left
1. Click **Review and save**, then **Save changes**

## Update Our Code

To complete our migration and rollout our new API, we need to add the new functionality to our code.

---

### Instructions

1. Open the [Code Editor](#tab-2), and locate the `/src/pages/api/checkout.ts` file
2. Replace lines 50-55 with the following code:
```js
//in this code, we are first retrieving the value for the enableStripe flag,
// then, if it returns true, running a function that creates a checkout session in stripe.
//If you want to see how that works, take a look at the `/src/utils/checkout-helpers.ts` file.

  if (req.method === 'POST') {
  const migrateToStripeApi = await ldClient.variation("migrate-to-stripe-api", jsonObject, false);

    if (migrateToStripeApi) {
      createCheckoutForStripe(req, res)
    }
  } if (req.method === 'GET') {
    const migrateToStripeApi = await ldClient.variation("migrate-to-stripe-api", jsonObject, false);

    if (migrateToStripeApi) {
      try {
        res.send("You are good to go!");
      } catch (error: any) {
        console.error(error.message);
      }
    } else {
      return res.json("the API is unreachable");
    }
  }
```
3. Save the file (^+S or ⌘+S)

## Pull It All Together

We previously turned off our **Updated Billing UI** flag due to errors, but if we refactored our code properly, it should work now. Since we've added the API component, we need to turn the **Updated Billing UI** flag back on.

> **Important:** Note that we're turning on the **Updated Billing UI** flag (from Lab 4), not the **Migrate to Stripe API** flag we just created. The **Updated Billing UI** flag controls the frontend feature, while the **Migrate to Stripe API** flag controls the backend API migration.

---

### Instructions

1. Switch back to the [LaunchDarkly](#tab-0) tab
1. From the left-hand navigation menu, click **Flags**
1. Click **Updated Billing UI** (this is the flag from the previous challenge, not the one we just created)
1. Click the On/Off toggle at the top left to turn the flag **On**.
1. Click **Review and save**, then **Save changes**.

Switch to the [Toggle Outfitters](#tab-1) tab, login as **ron**, **leslie**, **april**, or **andy** and you will now be able to click *Add to Cart* successfully!

Nice work! It's great to be able to quickly turn off a flag to fix errors. But what if a developer isn't the first to catch it? In the next challenge, we'll take a look at automating risk mitigation.
