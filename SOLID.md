## SOLID
[wikipedia](https://en.wikipedia.org/wiki/SOLID_(object-oriented_design))

### Single Responsibility Principle
A class should have one and only one reason to change, meaning that a class should have only one job.

Bad
- combine 2 things in 1 class

```js
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

Good
- split into 2 classes
- do one thing, do it well

```js
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

### Open/Closed Principle
Objects or entities should be open for extension, but closed for modification.

BAD
- `favor` is not open for extension
- no way to extend it without modifying the class

```js
var iceCreamFlavors=["chocolate","vanilla"];
var iceCreamMaker={
 makeIceCream (flavor) {
  if(iceCreamFlavors.indexOf(flavor)>-1){
   console.log("Great success. You now have ice cream.")
  }else{
   console.log("Epic fail. No ice cream for you.")
  }
 }
}
export default iceCreamMaker;
```


GOOD
- can extend `favor` via public funciton

```js
var iceCreamFlavors=["chocolate","vanilla"];
var iceCreamMaker={
 makeIceCream (flavor) {
  if(iceCreamFlavors.indexOf(flavor)>-1){
   console.log("Great success. You now have ice cream.")
  }else{
   console.log("Epic fail. No ice cream for you.")
  }
 }
 addFlavor(flavor){
  iceCreamFlavors.push(flavor);
 }
}
export default iceCreamMaker;
```

### Liskov Substitution Principle

Every subclass/derived class should be substitutable (可替换) for their base/parent class.

BAD
- `Square` inherated from `Rectangle` 
- in term of area, `Square` can be replaced by `Rectangle`
- bad abstraction

```js
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach((rectangle) => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

GOOD
- keep common/descriptoin in base class
- push distinct to derived classes

```js
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach((shape) => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

### Interface Segregation Principle (接口隔离)
no client should be forced to depend on methods it does not use. 只依赖需要的方法，不依赖不需要的方法

BAD
```js
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

GOOD

```js
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName('body'),
  options: {
    animationModule() {}
  }
});
```

### Dependency Inversion Principle
Always depends on interfaces not implementation, 模块间要通过抽象接口隔离开，而不是通过具体的类强耦合起来
- A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
- B. Abstractions should not depend on details. Details should depend on abstractions.

// BAD
```js
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(['apples', 'bananas']);
inventoryTracker.requestItems();
```

GOOD
```js
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach((item) => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ['HTTP'];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ['WS'];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(['apples', 'bananas'], new InventoryRequesterV2());
inventoryTracker.requestItems();
```