---
date: 2021-05-31
title: Wearables overview
aliases:
  - /wearables/wearables-overview/
  - /decentraland/wearables-overview/
description: An overview of wearable NFTs for Decentraland
categories:
  - Decentraland
type: Document
url: /creator/wearables/wearables-overview
weight: 1
---

<iframe id="emote-preview" style="width:100%;border:0;height:60vh;"></iframe>

<script>
  const profile = Math.ceil(Math.random() * 120)
document.getElementById("emote-preview").src = "https://wearable-preview.decentraland.org/?profile=default"+profile+"&transparentBackground&loop=true"

  function changeProfile() {
document.getElementById("emote-preview").contentWindow.postMessage({
  type: 'update',
  payload: { options: {
    profile: `default${Math.ceil(Math.random() * 120)}`
  } }
},'*')
return false
  }
</script>

<a onclick="changeProfile()" style="cursor: pointer">Refresh wearables ↺</a>

Wearables are the various items of clothing, accessories, and body features that can be used to customize the appearance of a Decentraland avatar. There is a selection of default wearables that are freely available to all avatars, but Decentraland also supports the creation and use of custom wearables that are represented by non-fungible tokens (or NFTs). This allows a finite amount of different wearables to be created, or minted, on the blockchain, similar to the LAND.

By default, Decentraland Wearables are minted on the Polygon/Matic sidechain so users can mint, buy, sell, or transfer items without having to pay gas fees.

**Wearables are organized into different categories, depending on what part of an avatar they modify:**

- Body shape (the shape of the entire avatar)
- Hat
- Helmet
- Hair
- Facial hair
- Head
- Upper body (e.g. jacket or shirt)
- Lower body (e.g. pants or shorts)
- Feet
- Skin

**Wearables can also include accessories that are applied to different parts of an avatar’s body:**

- Mask
- Eye wear
- Earring
- Tiara (a crown, or other accessory that sits on top of the head)
- Top-head (e.g. a halo, or other effect applied to the head)

For a detailed description of each category, and how items within each category interact or replace one another, see [Creating Wearables]({{< ref "/content/creator/wearables/creating-wearables.md" >}}).

### Collections & Items

Wearables exist as individual **items** that can be grouped into **collections**.

#### Items

Remember how wearables are represented by non-fungible tokens (NFTs)? Well, each wearable **item** can be minted to create multiple NFTs of that same item, to a limit according to the item’s rarity (the rarer the item, the fewer NFTs you can mint). Items are often referred to as the “representations” of the wearable.

Items cannot be bought or sold, only the NFTs that have been minted from items. Also, individual items cannot be published on their own; they must be part of a collection.

#### Collections

**Collections** exist to help creators organize and manage their items before publication.

For example, let’s say we create a blue t-shirt and a red t-shirt. We can create a new collection called “Summer T-Shirts”, and then add our two shirts to that collection. After publishing our collection, we can then mint copies of the red and blue shirts to share, trade, or sell.

The following documentation only covers the **Wearables Editor**: the tool used to upload, add metadata to, and publish custom items and collections of items.

For documentation covering other aspects of wearables, see the following resources:

- [Creating Wearables]({{< ref "/content/creator/wearables/creating-wearables.md" >}})
- [Publishing Wearables]({{< ref "/content/creator/wearables/publishing-wearables.md" >}})
- [Curation Committee]({{< ref "/content/creator/wearables/curation-committee.md" >}})
- [Editor User Guide]({{< ref "/content/creator/wearables/wearables-editor-user-guide.md" >}})

The following shared folder contains example wearables, base models, textures and other resources you can use:

- [Wearables Reference Models](https://drive.google.com/drive/u/1/folders/12hOVgZsLriBuutoqGkIYEByJF8bA-rAU)
