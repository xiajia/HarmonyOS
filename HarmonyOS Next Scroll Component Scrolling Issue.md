# HarmonyOS Next Scroll Component Scrolling Issue

## Problem Description
In HarmonyOS Next, developers encounter issues where the Scroll component fails to scroll even when content clearly exceeds the screen dimensions. Official documentation confirms this is a known behavior pattern.

Official HarmonyOS Scroll Documentation:  
[https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-scroll-V5](https://developer.huawei.com/consumer/cn/doc/harmonyos-references-V5/ts-container-scroll-V5)

## Working Implementation
	```javascript
	Scroll() {
	  Column() {}
	}.height('100%')
	```

## Non-Working Implementation
	```javascript
	Scroll() {
	  Column() {}.height('100%') // Problematic height setting
	}.height('100%')
	```

## Root Cause Analysis
	```javascript
	The Scroll component determines scrolling capability based on whether its direct child's height exceeds its own height. When set to 100%, the Column becomes the same height as the Scroll container, eliminating the height difference that triggers scrolling. Content overflowing from the Column is simply rendered off-screen without affecting Scroll's internal measurement.
	```

## Visual Explanation
	```
	+---------------------------+
	|        Scroll (100%)      |
	|   +-------------------+   |
	|   |    Column (100%)  |   | ← Same height = no scroll
	|   |                   |   |
	|   |  [Content Here]   |   |
	|   |                   |   |
	|   +-------------------+   |
	|                           |
	|  [Overflow Content]       | ← Rendered off-screen but not detected
	+---------------------------+
	```

## Solution Summary
	```javascript
	To resolve scrolling issues:
	1. Avoid setting direct children of Scroll to 100% height
	2. Ensure child elements have intrinsic height exceeding Scroll container
	3. Use percentage values carefully within scrollable contexts
	```