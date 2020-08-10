---
layout: post
title: Customizing the Model Browser - custom label, behavior, styling and data sources
date: 2020-01-12 00:00:00 +0300
description: Customizing the Model Browser has always been somewhat a puzzling experience for our developers so today let's sum up all our tricks up the sleeves to get geared up for you our customizing needs! # Add post description (optional)
img: kiyafuchiya.jpg # Add image post (optional)
fig-caption: # Add figcaption (optional)
tags: [] # add tag
---

Customizing the Model Browser has always been somewhat a puzzling experience for our developers so today let's sum up all our tricks up the sleeves to get geared up for you our customizing needs!

## **Before You Begin**

Wait after the object tree of the model is loaded to by subscribing to "Autodesk.Viewing.OBJECT_TREE_CREATED_EVENT" (and "Autodesk.Viewing.TOOLBAR_CREATED_EVENT" as well if working with UI) with your code in the callback. And be sure to call "NOP_VIEWER.modelstructure.createUI()" before you access Model Browser's UI elements.

## Custom Node Label and Model Name

First let's customize the node labels:

```
NOP_VIEWER.modelstructure.tree.delegates[0].getTreeNodeLabel= nodeId => {  
   //your code here, can use NOP_VIEWER.getProperties(nodeid, callback) to get model data ...   
   return yourCustomLabel
}
```

You can also override the name of the model here:

```
viewer.loadModel(svfURL, {
   modelNameOverride: "yourName"
   //...
}
```

## Accessing Node Element

If you need to access the DOM element of a node try:

```
const targetNodeElement = NOP_VIEWER.modelstructure.tree.elementsPool.find(e=>e.getAttribute('lmv-nodeid')==targetNodeId)
```

## Click Behavior

We can also customize the click behavior:

```
NOP_VIEWER.modelstructure.setClickConfig('click','onObject','isolate')
// or
NOP_VIEWER.modelstructure.clickConfig = {
    click: { onObject: ["isolate"] },
    clickCtrl: { onObject: ["toggleVisibility"] }   
 //...
}

// find all the possible actions below:
case "toggleOverlayedSelection": this.toggleOverlayedSelection(dbId, model); break; case "toggleMultipleOverlayedSelection": this.toggleMultipleOverlayedSelection(dbId, model); break;
case "selectOnly": this.viewer.select(dbId, model); break;
case "deselectAll": this.viewer.clearSelection(); break;
case "selectToggle": this.viewer.toggleSelect(dbId, model); break;
case "isolate": this.viewer.isolate(dbId, model); break;
case "showAll": this.viewer.showAll(); break;
case "focus": this.viewer.fitToView(); break; case "hide": this.viewer.hide(dbId, model); break;
case "show": this.viewer.show(dbId, model); break;
case "toggleVisibility": this.viewer.toggleVisibility(dbId, model); break;
```

And should you need to take full control of the callback simply try this:

```
viewer.modelstructure.onTreeNodeClick = function(tree, node, event){
//...
}

viewer.modelstructure.onTreeNodeRightClick= function(tree, node, event){
//...
}

viewer.modelstructure.onTreeNodeDoubleClick = function(tree, node, event){
//...
}
```

## Internationalization

Viewer comes with [i18n](https://www.npmjs.com/package/i18n) - so It is also possible to leverage its APIs to translate the node labels into different languages per the browser's locality settings:

```
swtichLanguage( lng ) {
    Autodesk.Viewing.i18n.setLng( lng, {
              localizeCategory:true, localize: true
         }, function( cb ) {  
              Array.prototype.forEach.call( document.querySelectorAll( 'div.model-div label' ), function( eachDOM ) {
                   const if_i18n = eachDOM.getAttribute( 'data-i18n' );
                   if( !if_i18n ) {
                       const text = eachDOM.innerHTML; eachDOM.setAttribute( 'data-i18n', text ); } });            
                       Autodesk.Viewing.i18n.localize();
                   });
}

const locales = {
           'en': {
                 'Materials and Finishes': 'Materials and Finishes', 'Materials': 'Materials',
                //...
           },
           'ja-JP': {
               'Materials and Finishes': '材料と仕上げ',
               'Material': '材料',
               //...
            }
};

Object.keys(locales).forEach(function(language) {
           Autodesk.Viewing.addResourceBundle( language, "allstrings", locales[language], true, true );
});
```

## Custom Data Sources

We have covered this topic before and here's a full fledged sample to get your started: https://github.com/yiskang/forge-au-sample/tree/master/model-structure

## Custom Styling

To customize title text simply try:

```
NOP_VIEWER.modelstructure.container.children[0].textContent = "YourCustomTitle"
```

And to add class names to a node try:

```
NOP_VIEWER.modelstructure.tree.delegates[0].getNodeClass = nodeId => {  
   // your code here
   return yourClassName
}
```

Also find a good read [here](https://forge.autodesk.com/blog/yet-another-merry-xmas-fix-misfiring-selection-and-model-browser-overflow-your-viewer) for your styling needs ... 
