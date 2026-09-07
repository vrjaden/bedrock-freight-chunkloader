# Bedrock Freight Chunk Loader

A lightweight behavior pack for Minecraft Bedrock Edition that keeps chest minecarts active while traveling outside normal simulation distance.

Designed for long-distance freight railways where chest minecarts would otherwise stop when entering unloaded chunks.

## Features

* Keeps chest minecarts active during long-distance travel
* Releases the cart's ticking area when it stops moving
* Uses Minecraft's `minecraft:tick_world` component
* 2-chunk ticking radius
* No custom entities
* No custom items or blocks
* No resource pack
* No scripts
* No permanent `/tickingarea`
* Vanilla chest minecart appearance and behavior

## How It Works

The pack modifies the vanilla `minecraft:chest_minecart` entity. An environment sensor checks `is_moving` and adds a component group containing:

```json
"minecraft:tick_world": {
    "radius": 2,
    "never_despawn": true
}
```

The sensor removes that group when the cart stops moving. Component-presence checks ensure the events only fire when the loading state needs to change, rather than reinitializing the ticking area repeatedly while traveling.

This is intended to keep nearby chunks ticking during travel beyond player simulation distance, while letting stationary carts unload normally. Movement detection and ticking-area release still need verification in the target Bedrock version.

## Installation

1. Download `FreightChunkLoader.mcpack` from Releases.
2. Open the file with Minecraft Bedrock Edition.
3. Enable the behavior pack on your world.
4. Test the pack before using it on an existing Realm or important world.

## Usage

No special items or commands are required.

Use vanilla chest minecarts normally. Moving chest minecarts activate chunk loading; stopped carts deactivate it.

## Important

This pack overrides the vanilla chest minecart behavior definition.

Every loaded chest minecart receives movement detection, but only moving carts activate their own ticking area.

A cart that stops outside other loaded areas cannot wake itself up after unloading. A nearby player or another chunk loader must load the area before the cart or station can operate again. Unattended unloading stations that stop carts need their own chunk loading to finish hopper transfers and restart the cart.

Keeping many moving chest minecarts loaded simultaneously may increase world/server processing.

Minecraft updates may also change the vanilla chest minecart definition. Compatibility should be tested after major Bedrock updates.

## In-game verification

1. Send a chest minecart along powered rails beyond player simulation distance and confirm it reaches the destination.
2. Stop a cart and confirm it no longer keeps the surrounding area ticking on its own.
3. With the area loaded by a player, restart the cart and confirm chunk loading resumes.
4. Save and reopen the world with a distant cart in transit to check restart behavior.

## Releases

The GitHub Actions workflow builds `FreightChunkLoader.mcpack` from `behavior_pack` and attaches it to a GitHub Release whenever a `v*` tag is pushed. Generated packages are not committed to the repository.

For each release, update both versions in `behavior_pack/manifest.json`, commit the changes, and tag that commit with the matching version (for example, `v1.0.1`). Push the branch and release tag to trigger publishing:

```sh
git push origin HEAD
git push origin v1.0.1
```
