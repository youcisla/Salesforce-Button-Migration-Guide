
# Moving Button "hada" in Salesforce Components

This README provides multiple approaches to move the button "hada" from Salesforce Component A to Component B in the `hna` div without losing its functionalities.

## Table of Contents
1. [Using JavaScript](#using-javascript)
2. [Using Salesforce Aura Components](#using-salesforce-aura-components)
3. [Using Salesforce Lightning Web Components (LWC)](#using-salesforce-lightning-web-components-lwc)
4. [Using CSS Flexbox/Grid](#using-css-flexboxgrid)
5. [Using Server-Side Rendering (Apex Controllers)](#using-server-side-rendering-apex-controllers)

## Using JavaScript

### Description
This approach uses JavaScript to dynamically move the button from Component A to Component B while retaining its functionalities.

### Code Example

```html
<!-- Original HTML Structure -->
<div class="a">
    <div class="c">
        <div class="d">
            <button class="hada" type="submit"></button>
        </div>
    </div>
</div>

<div class="comp">
    <div class="b">
        <div class="f">
            <div class="g"></div>
        </div>
    </div>
    <div class="e">
        <div class="q">
            <div class="hna"></div>
        </div>
    </div>
</div>
```

```javascript
// JavaScript to move the button
document.addEventListener('DOMContentLoaded', function() {
    var button = document.querySelector('.a .hada');
    var targetDiv = document.querySelector('.comp .e .hna');
    if (button && targetDiv) {
        targetDiv.appendChild(button);
    }
});
```

## Using Salesforce Aura Components

### Description
Use Aura attributes and conditional rendering to move the button between components.

### Code Example

#### Component A

```html
<aura:component>
    <aura:attribute name="moveButton" type="Boolean" default="false" />
    <div class="a">
        <div class="c">
            <div class="d">
                <aura:if isTrue="{!not(v.moveButton)}">
                    <button class="hada" type="submit"></button>
                </aura:if>
            </div>
        </div>
    </div>
</aura:component>
```

#### Component B

```html
<aura:component>
    <aura:attribute name="moveButton" type="Boolean" default="false" />
    <div class="b">
        <div class="f">
            <div class="g"></div>
        </div>
    </div>
    <div class="e">
        <div class="q">
            <div class="hna">
                <aura:if isTrue="{!v.moveButton}">
                    <button class="hada" type="submit"></button>
                </aura:if>
            </div>
        </div>
    </div>
</aura:component>
```

## Using Salesforce Lightning Web Components (LWC)

### Description
Use JavaScript properties to control the rendering location of the button.

### Code Example

#### Component A

```html
<template>
    <div class="a">
        <div class="c">
            <div class="d">
                <template if:false={moveButton}>
                    <button class="hada" type="submit"></button>
                </template>
            </div>
        </div>
    </div>
</template>
```

#### Component B

```html
<template>
    <div class="b">
        <div class="f">
            <div class="g"></div>
        </div>
    </div>
    <div class="e">
        <div class="q">
            <div class="hna">
                <template if:true={moveButton}>
                    <button class="hada" type="submit"></button>
                </template>
            </div>
        </div>
    </div>
</template>

<script>
import { LightningElement, api } from 'lwc';

export default class ComponentB extends LightningElement {
    @api moveButton = false;
}
</script>
```

## Using CSS Flexbox/Grid

### Description
Use CSS to visually move the button without altering the DOM structure.

### Code Example

```html
<style>
.a .hada {
    position: absolute;
    top: 0;
    left: 0;
}

.comp .e .hna {
    position: relative;
}
</style>

<div class="a">
    <div class="c">
        <div class="d">
            <button class="hada" type="submit"></button>
        </div>
    </div>
</div>

<div class="comp">
    <div class="b">
        <div class="f">
            <div class="g"></div>
        </div>
    </div>
    <div class="e">
        <div class="q">
            <div class="hna"></div>
        </div>
    </div>
</div>
```

## Using Server-Side Rendering (Apex Controllers)

### Description
Use Apex controllers to determine where to render the button based on server-side logic.

### Code Example

#### Apex Controller

```apex
public class ButtonController {
    @AuraEnabled
    public static Boolean shouldMoveButton() {
        return true; // Logic to determine if the button should be moved
    }
}
```

#### Aura Component

```html
<aura:component controller="ButtonController" implements="force:appHostable,flexipage:availableForAllPageTypes,force:hasRecordId">
    <aura:attribute name="moveButton" type="Boolean" />
    <aura:handler name="init" value="{!this}" action="{!c.doInit}" />

    <div class="a">
        <div class="c">
            <div class="d">
                <aura:if isTrue="{!not(v.moveButton)}">
                    <button class="hada" type="submit"></button>
                </aura:if>
            </div>
        </div>
    </div>

    <div class="comp">
        <div class="b">
            <div class="f">
                <div class="g"></div>
            </div>
        </div>
        <div class="e">
            <div class="q">
                <div class="hna">
                    <aura:if isTrue="{!v.moveButton}">
                        <button class="hada" type="submit"></button>
                    </aura:if>
                </div>
            </div>
        </div>
    </div>
</aura:component>
```

#### Component Controller

```javascript
({
    doInit: function(component, event, helper) {
        var action = component.get("c.shouldMoveButton");
        action.setCallback(this, function(response) {
            var state = response.getState();
            if (state === "SUCCESS") {
                component.set("v.moveButton", response.getReturnValue());
            }
        });
        $A.enqueueAction(action);
    }
});
```

---

This README file provides detailed instructions and code examples for each approach to move the button "hada" from Component A to Component B in Salesforce without losing its functionalities. Choose the approach that best fits your requirements and environment.
