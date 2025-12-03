---
slug: creating-your-first-flag
id: nsksxnaudha7
type: challenge
title: Creating Your First Feature Flag
teaser: Let's create new feature flag we can use to control the release of our new
  feature
notes:
- type: text
  contents: Did you know that LaunchDarkly can reduce rollbacks from hours to seconds?
    There's no need to redeploy or deal with extended outages. Simply flip a switch
    to return to the last known good state!
tabs:
- id: stnyezn9ofpz
  title: LaunchDarkly
  type: browser
  hostname: launchdarkly
- id: sbrvy7na7sch
  title: Toggle Outfitters
  type: service
  hostname: workstation
  port: 3000
- id: dcvyxrfcfkjn
  title: Code Editor
  type: service
  hostname: workstation
  port: 8080
difficulty: basic
timelimit: 600
lab_config:
  default_layout_sidebar_size: 0
---

# Lab 1: Getting Started with the Toggle Outfitters App

Toggle Outfitters is a retailer whose online presence is in need of an update. We're going to help modernize their website, and we're going to minimize downtime while we're at it.

The first thing we want to do is release our minimally viable product. Currently, we have a "Coming Soon" page, but we want to at least list our products availability.

## Create a Flag

Let's begin by creating a feature flag. This flag won't do much right now, but over the next few challenges, we'll incorporate this and other flags into an application and watch LaunchDarkly in action.

> **Note:** LaunchDarkly "dogfoods" our own software, which means we use our own feature flags to control the LaunchDarkly application itself!
>
> You may see one of two different UIs for creating flags—either a modal dialog or a full page. Both are valid, and this is a great example of how feature flags enable gradual rollouts of new features.
>
> Follow the instructions below based on which UI you see.

---

### Instructions

<details>
<summary><strong>Click for Modal Dialog Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Release Updated Storefront
```
4. In the bottom left corner, click the **No template** button and change it to **Release**. This template will allow you to set variation names after creating the flag.
5. Click **Create flag** in the lower right-hand side of the modal.
6. After the flag is created, select the **Variations** tab at the top of the page.
7. Update the variation names:
   - First variation **Name**: 
```js
Store Enabled
```
   - Second variation **Name**:
```js
Store Disabled
```

Congratulations! You've created your first flag and you're ready to proceed to the next challenge!

</details>
&nbsp;
<details>
<summary><strong>Click for Full Page Instructions</strong></summary>

1. From the left-hand navigation menu, click **Flags**
2. Click the **Create flag** button in the upper right-hand corner
3. For **Name**, enter:
```js
Release Updated Storefront
```
4. Under **Variations**:
   a. First **Name**:
```js
Store Enabled
```
   b. Second **Name**:
```js
Store Disabled
```
5. Click **Create flag** in the lower right-hand side of the screen.

Congratulations! You've created your first flag and you're ready to proceed to the next challenge!

</details>
