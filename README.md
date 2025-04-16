# 🎛️ Understanding Parameters in Bicep — Making Your Templates Smart & Flexible

> **Bitesize Lesson 🍬**  
Parameters are the **power-up** your Bicep templates need to go from “one-size-fits-all” to “tailored-for-any-situation.”  
This explainer breaks down what parameters are, how to use them like a pro, and how to keep your templates *friendly for both humans and automation*.  
Also, hey future-me 👋 — this is why we made the parameters so configurable and tidy.

---

## 📦 What’s a Parameter?

A **parameter** is a value you pass *into* your Bicep template when deploying — kind of like filling out a form before placing an online order. 🛒  
It makes your template **dynamic**, **reusable**, and **customizable** — without changing the code itself.

### 👇 Basic Example

```bicep
param environmentName string
```

This lets someone pass in a value like `"dev"` or `"prod"` when deploying. Use it inside your template like:

```bicep
name: 'app-${environmentName}'
```

Boom — flexible naming. 🎯

---

## 🛑 Hardcoding Is Out — Parameters Are In

Hardcoded names and locations = 😬  
Dynamic values using parameters = 😍

You can even **set default values** if someone doesn’t provide one:

```bicep
param environmentName string = 'dev'
param location string = resourceGroup().location
```

Now your template will:
- Default to `dev` if no environment is specified
- Use the current resource group’s region

---

## 🧠 Parameter Types You Can Use

Here’s what you can pass into your templates:

| Type     | What It’s For                           |
|----------|------------------------------------------|
| `string` | Text, names, regions, SKUs               |
| `int`    | Numbers (like instance count)            |
| `bool`   | True/false flags (like enableLogging)    |
| `array`  | Lists of things (like regions or emails) |
| `object` | Grouped key/value data (like SKUs, tags) |

---

## 🧱 Using Objects and Arrays

### 🧰 Object Example: App Service Plan SKU

```bicep
param appServicePlanSku object = {
  name: 'F1'
  tier: 'Free'
  capacity: 1
}
```

Use it like this:

```bicep
sku: {
  name: appServicePlanSku.name
  tier: appServicePlanSku.tier
  capacity: appServicePlanSku.capacity
}
```

### 🏷️ Object Example: Resource Tags

```bicep
param resourceTags object = {
  Environment: 'Test'
  Owner: 'Team Rocket'
}
```

Then reuse across multiple resources:

```bicep
tags: resourceTags
```

### 📋 Array Example: Multiple Locations

```bicep
param cosmosDBAccountLocations array = [
  { locationName: 'eastus' }
  { locationName: 'westeurope' }
]
```

Use it like:

```bicep
properties: {
  locations: cosmosDBAccountLocations
}
```

---

## 🧩 Add Constraints with Decorators

### 🎯 Restrict Values Using `@allowed`

```bicep
@allowed(['P1v3', 'P2v3', 'P3v3'])
param appServicePlanSkuName string
```

Now users can only pick from those SKUs — perfect for **production enforcement**.

### 🔐 Limit Length (for things like storage account names)

```bicep
@minLength(5)
@maxLength(24)
param storageAccountName string
```

### 📈 Set Ranges for Numbers

```bicep
@minValue(1)
@maxValue(10)
param appServicePlanInstanceCount int
```

No more rogue deployments spinning up 50 VMs 🚨

---

## 📝 Add Descriptions with `@description`

Make your templates **self-explanatory** and user-friendly:

```bicep
@description('List of Azure regions for the Cosmos DB account.')
param cosmosDBAccountLocations array
```

The Azure Portal will even show this description if someone uses your template there. 💬

---

## ⚠️ Tips & Best Practices

✅ **Use parameters for things that vary per deployment**  
❌ Don’t overdo it — too many parameters = confusion  
✅ Use **objects** for complex settings (like SKUs or tags)  
✅ Use **defaults** for dev-friendly values (like free SKUs)  
✅ Add constraints with `@allowed`, `@minLength`, etc.  
✅ Always add **helpful descriptions** for clarity  
🚫 Never output or hardcode **secrets** — use Key Vault instead!

---

## 🎯 TL;DR Recap

- ✍️ **Declare parameters** using `param name type`
- 🔁 Make them **dynamic** with default values and expressions
- 🧱 Use `object` and `array` types to group related data
- 🎯 Control inputs with decorators like `@allowed` and `@minLength`
- 🗣️ Document with `@description` for human-friendliness
- 💡 Use parameters to make templates *flexible*, *safe*, and *reusable*

---

Now your Bicep templates are no longer rigid blocks — they’re flexible, dynamic blueprints ready to be used over and over again. 🧠💥

Well done, future-me. This is why you parameterized everything so cleanly. 🙌
