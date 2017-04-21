# \<polux-mediator\>

## Introduction

In Polymer the comunication between components is one of the main problems at the moment of create complex apps.

The disconnection  between brother components, the obligation use of mediator pattern...
With polux we want to make all components accesible from any other component without matter in what level of the DOM are.

Polux-mediator will have all the instances component references in the app allowing the access to other components via events.

## How it works

polux-mediator will stay at the top level of our app jerarchy and every of our app components will dispatch in his ready lifecycle callback an init event passing his reference at the detail.

This will save it in our polux-mediator component and make possible the access dispatching an use-component event. This event will have a parameters that will explain later on.

Depend on the parameters we pass it the component will have a comunication type or another.

## Usage

Install the component

```bash
  $ bower install --save https://github.com/Borjagodoy/polux-mediator.git
```

Import the component to your app

```html
  <link rel="import" href="/bower_components/polux-mediator/polux-mediator.html">
```

Use component in your index.html app

```html
  <polux-mediator></polux-mediator>
```

In this first version you will have to put in the ready lifecycle callback of your component the event that will registry at the polux

```js
  ready() {
    super.ready();
    this.dispatchEvent(new CustomEvent('init', {
      bubbles: true,
      composed: true,
      detail: {
          reference: this
      }
    }));
  }
```

For the use of components storage at the polux you have three comunication ways:

 1. Execute a function from another component
```js
this.dispatchEvent(new CustomEvent('use-component', {
   bubbles: true,
   composed: true,
   detail: {
       componentToUse: 'name-of-component-to-use',
       task: 'name-of-function-to-use',
       value: 'function-parameter-to-use' // not required
   }
  }));
```
 2. Modify a property of another component
```js
this.dispatchEvent(new CustomEvent('use-component', {
    bubbles: true,
    composed: true,
    detail: {
        componentToUse: 'name-of-component-to-use',
        property: 'name-of-property-to-modify',
        value: 'new-property-value'
    }
  }));
```
  3. Modify the property of the component who dispatch the event using another component function
```js
this.dispatchEvent(new CustomEvent('use-component', {
    bubbles: true,
    composed: true,
    detail: {
        componentFrom: 'component-dispatch-event',
        componentToUse: 'name-of-component-to-use',
        task: 'name-of-function-to-use',
        propertyFrom: 'name-of-property-from-componentFrom-to-modify',
    }
  }));
```  
