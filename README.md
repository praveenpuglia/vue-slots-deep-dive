# Vue Slots - Deep Dive

Like what you read? Give it a ⭐️. 

## introduction

Let's say we are asked to make two cards. 

1. A feature card : Where we render the name of the feature and its description.
2. An author card: Where we render the name of the author and their bio.

The wizards we are, we go on to make a generic **Card** component which takes `title` and `content` as props and renders both of our cards. We go home and sleep well.

That component looks like this.

```vue
<template>
  <div class="card">
    <div class="card-title">
      {{title}}
    </div>
    <div class="card-content">
      {{content}}
    </div>
  </div>
</template>

<script>
export default {
  props: ["title", "content"]
};
</script>
```

The usage in the parent / consumer component looks like this.

```html
<card title="John Doe" content="Award winning author."></card>
```

But here's the problem. _Our card only works as long as both `title` and `content` are plain strings_. Anything a tad bit complicated than that and it breaks down. 

This is where **slots** come into picture.

## what are slots?

Slots are like post offices. Putting something in a slot is like asking that something to go to a particular address. 

- If there is a place with that address, it goes there and stays. 

- If the address is not specified, it ends up in a place with a pre-defined address(if it exists) or gets lost.

So with slots, our **Card** component now looks like this.

```vue
<template>
  <div class="card">
    <div class="card-title">
      <slot name="title"></slot>
    </div>
    <div class="card-content">
      <slot></slot>
    </div>
  </div>
</template>
```

and it is used this way.

```html
<card>
  <template v-slot:title>John Doe</template>
  <template v-slot>Award winning author.</template>
</card>
```

What we just did is we

- asked `John Doe ` to go to the place with the address **title** in our card component.
- asked `Award winning author.` to go... but without an address. So it went to **default** address in our card. The default slot. The slot without a name on it.

Our rendered card looks exactly the same but hey! now we can pass in all the HTML in the world to those slots and they'll render them.

```html
<card>
  <template v-slot:title>
  	<h1>John Doe</h1>
  </template>
  <template v-slot>
    <p>Award winning author.</p>
  </template>
</card>
```

Amazing!

## named slots

As the _name_ suggests, these slots are **named**. Don't kill me.

They allow us to pass an entire DOM tree to a designated place in our card component. In our previous example, we sent the `<h1>John Doe</h1>` to render within a slot named **title**.

## slot usage order

The cool thing about slots , out of many, is that it does't matter in what order we use those slots. They will always land up in the specified place. So we could use the **title** slot after the **default** slot and it would still work as expected.

```html
<card>
  <template v-slot>
    <p>Award winning author.</p>
  </template>
  <template v-slot:title>
  	<h1>John Doe</h1>
  </template>
</card>
```

## default slot shorthand

We have been putting everything that needs to go into the default slot within a `<template>` tag with the `v-slot` attribute. But we really don't need to do that. 

Any content we put inside our card component will get _slotted_ into the default slot, by default(don't kill me again!). If a default slot is not defined, that content won't even be rendered. So the one below is the same as the ones above.

```html
<card>
  <template v-slot:title>
  	<h1>John Doe</h1>
  </template>
  <p>Award winning author.</p>
</card>
```


> FUN FACT : The slot component and api is inspired by the Web Component API's draft spec.


## do slots _wrap_ what goes in them?

Nope!

Slots are renderless components. They don't render their own markup. So what you put inside a slot is what gets into the final markup. There is no wrapper `<div>` or a `<slot>` around our _slotted_ content in the final markup. Go Inspect!



## accessing *used* slots in the scripts

In our card component, we can access all the slots that have been used in the parent / consumer component using `this.$slots`

In our case, it looks like this.

```js
{
  default: [/*Array of VNode*/],
  title: [/*Array of VNode*/]
}
```

If we didn't use the **title** slot in our parent, it wouldn't show up here.

## conditionally rendering slots

Using the previous knowledge, we can conditionally render slots *only if they were used*.

**Example** - It doesn't make sense for us to render the `<div class='card-title'></div>` in card component if the title slot wasn't even used.

Let's change our card component's template to reflect the same.

```vue
<template>
  <div class="card">
    <div class="card-title" v-if="$slots.title">
      <slot name="title"></slot>
    </div>
    <div class="card-content">
      <slot></slot>
    </div>
  </div>
</template>
```

## that's all?

Is that everything we need to know about slots? Are we done? No!

We have just barely scratched the surface of the slots. So far, we have used so called _simple slots_ or _normal slots_. 

However, the fun begins with scoped slots.

## Introduction : Scoped Slots

WIP!
