---
slug: recover-from-failures
id: 8twuyjs6k2ro
type: challenge
title: Recovering From Release Failures
teaser: Learn how to avoid rollbacks and data integrity issues
notes:
- type: text
  contents: Our initial version of our website is pretty basic. It allows visitors
    to reach out to us to place an order, but now we want to enable customers to order
    directly from our website without the need to talk to a representative.
tabs:
- id: ozu1tzikycc3
  title: LaunchDarkly
  type: browser
  hostname: launchdarkly
- id: s2g5umo4jrca
  title: Toggle Outfitters
  type: service
  hostname: workstation
  port: 3000
- id: si2fve1qnu0j
  title: Code Editor
  type: service
  hostname: workstation
  port: 8080
difficulty: basic
timelimit: 600
lab_config:
  default_layout_sidebar_size: 0
---

# Lab 4: Recovering From Release Failures

Instead of building our own shopping cart, let's use an existing framework. This will make it very easy to implement and maintain.

## Create a Flag

Let's start by creating a new flag to handle our new billing user interface.

> **Remember:** You may see either a modal dialog or a full page UI when creating flags. Follow the instructions below based on which UI you see.

---

### Instructions

<details>
<summary><strong>Click for Modal Dialog Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Updated Billing UI
```
4. In the bottom left corner, click the **No template** button and change it to **Release**. This template will allow you to set variation names after creating the flag.
5. Click **Create flag** in the lower right-hand side of the modal.
6. After the flag is created, select the **Variations** tab at the top of the page.
7. Update the variation names:
   - First variation **Name**: 
```js
Enable Stripe
```
   - Second variation **Name**:
```js
Self-hosted Form
```

</details>
&nbsp;
<details>
<summary><strong>Click for Full Page Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Updated Billing UI
```
4. Under **Configuration**, select **Release**
5. Under **Variations**:
   a. First **Name**:
```js
Enable Stripe
```
   b. Second **Name**:
```js
Self-hosted Form
```
6. Click **Create flag** in the lower right-hand side of the screen.

</details>


The settings we've created for this flag will prevent our new feature from being seen by everyone--whether the flag is on or off. But we do want our developers to begin testing the new feature, so let's add a targeting rule which just allows those in the Developers segment to use the new feature.

---

### Instructions

1. Click **+ Add rule** and choose **Target segments**
1. From the **Segments** dropdown, select *Developers*
1. From the **Rollout** dropdown, select *Enable Stripe*
1. Toggle the On/Off flag to **On** in the upper left
1. Click **Review and save**, then **Save changes**


## Add the Code

Within our application, we need to implement the new feature that will be controlled via this **Updated Billing UI** feature flag.

---

### Instructions

1. Open the [Code Editor](#tab-2), and locate the `/src/components/inventory.tsx` file.
2. Scroll to **line 131** and locate the `<ReserveButton />` object. Replace the entire `<ReserveButton />` code block (lines 131-138) with the following:

```js
{
  updatedBillingUi ? (
    <AddToCartButton
      product={product}
      errorTesting={errorTesting}
      clickHandler={addToCartClickHandler}
    />
  ) : (
    <ReserveButton
      setHandleModal={setHandleModal}
      handleModal={handleModal}
      handleClickTest={handleClickTest}
      updateField={updateField}
      formData={{ name, email }}
      onButtonClick={onButtonClick}
    />
  )
}
{
  updatedBillingUi && (
    <ErrorDialog errorState={errorState} setErrorState={setErrorState} />
  )
}
```
3. Save the file (^+S or ⌘+S, though it should autosave)

Switch over the the [Toggle Outfitters](#tab-1) tab. Login as **ron**, **leslie**, **april**, or **andy**, and you will see the **Reserve Yours** button has changed to **Add to cart**.

## Test and Recover

Make sure you're still logged in as **ron**, **leslie**, **april**, or **andy**, then click the **Add to Cart** button on one of the items.

Whoops! It looks like there's an error in our system!

No matter how much testing we do, sometimes buggy code can make it into our production environment. Fortunately, LaunchDarkly allows you to recover in just seconds!

---

### Instructions

1. Go back to the [LaunchDarkly](#tab-0) tab.
1. Toggle the On/Off flag to **Off** in the upper left
1. Click **Review and save**, then **Save changes**

Review the [Toggle Outfitters](#tab-1) site once more, and even though you're logged in as a developer, we can no longer see the new **Add to Cart** function since the entire feature is now off.

Great work! In the next challenge, we'll see how we can further automate recoveries and make migrations a cinch!
