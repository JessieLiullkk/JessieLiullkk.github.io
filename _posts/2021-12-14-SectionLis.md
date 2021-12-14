---
title: React Native - SectionList, FlatList
tags: React-Native, SectionList, FlatList
category: /小书匠/日记/2021-12
renderNumberedHeading: true
grammar_cjkRuby: true
---

# SectionList
```
sectionListRef.current?.scrollToLocation({
	sectionIndex: 0,
	itemIndex: 1,
 });
```
![enter description here](https://raw.githubusercontent.com/JessieLau-CT/images/main/小书匠/1639464471249.png)


##### Why `scrollToLocation` function cannot work?
```markdown
## Root cause
* scrollToLocation should be used with getItemLayout and onScrollToIndexFailed, otherwise it cannot scroll to locations outside the render window.
* If each element hasn't a fixed height, it cannot scroll to specific sectionIndex or itemIndex.
  
## scrollToLocation params
* animated (default: true): Whether the list should do an animation while scrolling
* itemIndex: Index within section for the item to scroll to. Required.
* sectionIndex:  Index for section that contains the item to scroll to. Required.
* viewOffset:  A fixed number of pixels to offset the final target position, e.g. to compensate for sticky headers.
* viewPosition:  A value of 0 places the item specified by index at the top, 1 at the bottom, and 0.5 centered in the middle.
```

##### How to set `itemIndex` in scrollToLocation?
```markdown
_SectionList contains Header and Footer, so no matter whether Header or Footor has been set. We should also count it._
> Example
const dataSource = [{title: "Fruit", data: ["apple","banner"]},{title: "Animal", data: ["panda","pig"]}]
Index0: title - "Fruit"
Index1: "apple"
Index2: "banner",
Index3: this first section footer
Index4: title - "Animal"
```

##### Without ScrollToLocation, how to scroll to specified position by SectionList?
```markdown
1. sectionListRef.current?.getScrollResponder()?.scrollTo({y: positionY});
2. sectionListRef.current?.scrollToLocation({
	sectionIndex: 0,
	itemIndex: 0,
	viewOffset: -positionY,
 });
```
