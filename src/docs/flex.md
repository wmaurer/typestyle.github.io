Having well defined semantics of *how the layout will happen* is important for a maintainable layout system. The more variables you throw into the semantics the less reliable the layout becomes because *you need to compile the final result in your brain* in order to maintain the layout. We will cover the *concept* of a flexible layout system inspired by a *subset* of CSS flexbox.

## Root
We will look at `flex`ible children and `content` children. These concepts exist inside a *`root`*. The root is simply the *container* that is used as a point of reference for these children.

> A general purpose `csstips.flexRoot` exists.

## Flex
> A *flex* container `csstips.flex` has the same size as its parent.

Consider the following layout:

![](/images/book/flex/flex-small.png)

Here the content *takes up all the available space offered by the parent*. If the parent offers more space, the child takes it gladly.

![](/images/book/flex/flex-large.png)

The space taken by the child (content) is what is available to its children. No more, no less.

Such a child is called *flex* (`csstips.flex`).

## Content
> A *content* (`csstips.content`) child determines its size based on the size of its content

In the previous example the child *flexed* into the parent. The only other concept we need for a child is that of *content*. **A *content* child determines its size based on the size of its content**. That is all the space it takes up in the parent. This is shown below where if the parent is too big the rest of the space is unused:

![](/images/book/flex/content-unused.png)

If the parent is too small the content will overflow:

![](/images/book/flex/content-overflow.png)

> If you want the parent flex container to scroll for content just mixin `csstips.scroll`.

## Root (redux)

In CSS flexbox the concept of `root` does not exist without combining it with the concept of *flex direction*.

> The general purpose `csstips.flexRoot` is just an alias (more semantic name) for `csstips.horizontal`.

A root has a default *main axis* of `horizontal`. This is axis in which the children are layed out. In the *cross axis* the children are by default forced to `flex`.

So there are really two roots:
* `csstips.horizontal`: Lays out children horizontally based on `content` and `flexes` them vertically.
* `csstips.vertical`: Lays out children vertically based on `content` and `flexes` them horizontally.

Here is an example of a `csstips.horizontal` layout with `content` children.

![](/images/book/flex/horizontal.png)

Here is an example of a `csstips.vertical` layout with `content` children.

![](/images/book/flex/vertical.png)

# Examples
We've seen four types of containers : `csstips.vertical`, `csstips.horizontal`, `csstips.flex`, `csstips.content`. The next step is to practice with real world examples

## Vertical Example
Consider the following layout:

![](/images/book/flex/vertical-small.png)

Up front we know that the root is `csstips.vertical`. Assume that we want the `body` section to *flex* i.e. as the root becomes larger it expands to consume the remaining space, while `header` and `footer` remain sized based on their content:

![](/images/book/flex/vertical-large.png)

In our lingo the `root` here is `csstips.vertical` that has three children:

* header: `csstips.content`
* body: `csstips.flex`
* footer: `csstips.content`

Visually: 

![](/images/book/flex/vertical-solution.png)

> As mentioned before the children are going to automatically flex in the cross dimension (in this case horizontal).

## Multiple Flex Children
The `flex` children actually share the *remainder* of the space left in the `root` after all the `content` children take up the space they need. This is shown below:

![](/images/book/flex/multiple-flex-small.png)

If the parent becomes bigger the `flex` children share all the space that remains after the `content` children take their share:

![](/images/book/flex/multiple-flex-large.png)

Actually a flex child can decide a *flex scaling factor* (`csstips.flex1`,`csstips.flex2` ... `csstips.flex12`) to divide up the remaining space. E.g. a 1:2 ratio can easily be achieved:

![](/images/book/flex/multiple-flex-scaling.png)

The remainder space is divided into `3` (`1 + 2`) equal parts with `1` part going to a `A` and `2` parts going to `B`.

## Horizontal Example

Consider the layout:

![](/images/book/flex/horizontal-small.png)

Where we want to `body` to grow:

![](/images/book/flex/horizontal-large.png)

We can see intutively that it is a *horizontal* container with *content* sidebars and *flex* body.  

* root: `csstips.horizontal`
* sidebar: `csstips.content`
* body: `csstips.flex`
* sidebar: `csstips.content`

This example should have been fairly obvious and was designed to give you a hands on experience 🌹. Visually:

![](/images/book/flex/horizontal-solution.png)

## Arbitrary Layout
Consider this layout:

```
----------------------------------------------------
|                     HEADER                       |
----------------------------------------------------
|           |                          |           |
|  SIDEBAR  |          BODY            |   SIDEBAR |
|           |                          |           |
----------------------------------------------------
|                     FOOTER                       |
----------------------------------------------------
```
This is actually a layout used by lots of applications. If you think about it, its just a nesting of concepts you already know `csstips.vertical`,`csstips.horizontal`, `csstips.flex`, `csstips.content`.

In fact its a combination of the first example layout (`csstips.vertical`):

```
------------------------------------
|             HEADER               |
------------------------------------
|                                  |
|              BODY1               |
|                                  |
------------------------------------
|             FOOTER               |
------------------------------------
```

Where the `body1` is itself a `csstips.horizontal` containing the `sidebar`s and `body`:

```
-----------------------------------------------
|                    HEADER                   |
-----------------------------------------------
|             |                 |             |
|   SIDEBAR   |       BODY      |    SIDEBAR  |
|             |                 |             |
-----------------------------------------------
|                    FOOTER                   |
-----------------------------------------------
```
Easy right! Here is a demo for you to play with:

```play
const bg = (backgroundColor) => ({backgroundColor});

<div className={style(csstips.fillParent, csstips.vertical)}>
  <div className={style(csstips.content,csstips.height(50), bg(csx.lightskyblue))}>
    Header
  </div>
  <div className={style(csstips.flex, csstips.horizontal)}>
    <div className={style(csstips.content, csstips.width(100), bg(csx.lightpink))}>
      Sidebar
    </div>
    <div className={style(csstips.flex, bg(csx.darkorange))}>
      Body
    </div>
    <div className={style(csstips.content, csstips.width(100), bg(csx.limegreen))}>
      Sidebar
    </div>
  </div>
  <div className={style(csstips.content,csstips.height(50), bg(csx.lightskyblue))}>
    Footer
  </div>
</div>
```

[](TODO: # Scrolling)

[](TODO: # Scrolling a sub child)

[](TODO Components: We've covered enough of layout to allow you to create basic layouts quite easily)

[](TODO: Overlays)

[](TODO: Menu)
