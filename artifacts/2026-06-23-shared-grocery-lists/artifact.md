# Shared grocery lists

A shared grocery list feature for the recipe app, aimed at households who cook together but shop separately. The core flow: one person adds items (including straight from a recipe), and another checks them off at the store in real time. Today users work around the gap by screenshotting lists or texting each other, which quickly goes stale.

## Problem

Recipe app users keep asking to share shopping lists with their household. Households who cook together but shop separately have no good way to coordinate: they screenshot lists or text items back and forth, and those copies go stale as soon as someone buys or adds something.

## Users & scenarios

- **Primary users:** people cooking for a household who shop separately.
- **Core scenario:** one person adds items from a recipe; another person checks them off at the store, in real time.

## Goals / non-goals

**Goals**
- Real-time sync of the list across household members.
- Add-from-recipe: pull a recipe's ingredients onto the list.
- Check-off at the store, visible live to others.

**Non-goals (v1)**
- Price tracking.
- Store inventory.
- Multiple lists per household — start with a single shared list.

## Requirements

User stories:

1. As a household member, I can invite others to our shared list via a link.
2. As a household member, I can add a recipe's ingredients to the list in one tap.
3. As a household member, I see others' check-offs update live.

## Decisions & trade-offs

- **Last-write-wins on conflicts (v1).** Real-time sync adds infra cost and offline-merge complexity. To keep v1 simple, conflicts are resolved by last-write-wins rather than a richer merge strategy.

## Open questions & risks

- **Member leaving the household:** when a member leaves, do their previously added items stay on the list?
- **Link over-sharing / moderation:** if a share link is spread too widely, do we need any moderation or access controls?
