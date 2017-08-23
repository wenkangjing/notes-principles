How To Design A Good API and Why it Matters
-------------------------------------------

### Getting the requirements for your API:
- extract "true requirements" from your customers (*often, customers give you solutions as requirements, when they should be giving you a problem*)
- extract requirements in the form of use cases (i.e. "as a shopper, i want to be able to add items to my cart")
- create a **small specification** (about one page) and create it fast. Then present it to your customers for feedback, modify, and repeat. In other words, create the specification in an "iterative" manner (with constant feedback from your customer)

### Writing the API:
- make your classes/functions small and cohesive. **If its hard to name, its doing too much.**
- name, name, name. **Naming is everything**. Better names, makes your API easier to learn/use.
- use the conventions/idioms/patterns that are common in whatever language/platform your API is meant to be used on. This reduces the amount your clients have to learn in order to use your API.
- document every single class/function. For a class, what does it represent? For a function, what does it do, what are its pre/post conditions and does it have any side effects?
- **keep everything as private as possible** (private classes/member functions/fields)
- design your API **with change in mind**. You will not get it perfect the first time. Try to keep that in mind when designing your API and you're more likely to design it in such a way that it can evolve/adapt later when needed.
- "when in doubt, leave it out", if you're not sure about adding something to your API, don't. You can always add later when you are SURE. If you add it now, when your not sure, you may get it wrong, now your clients are using that wrong API and you can't change it. **Remember, you can always add to your API, but you can't remove without breaking client code.**
- keep performance in mind, but don't sacrifice readability for it.
- keep classes **as immutable as possible**. Even if you can't make a class completely immutable, make it as immutable as you can.
- only inherit when is- a relationship exists.
- if a class is meant to be inherited from, document it
- if a class is not meant to be inherited from, prohibit it (some languages let you enforce this, in others, you can just write something like "Do not inherit" in the documentation) (the reason for this is that inheritance violates encapsulation/information hiding)
- *don't make your client do anything you can do for them* (this reduces the amount of boilerplate code your client will have)
- don't violate principle of least astonishment (clients on your platform are used to certain behavior in certain situations, go along with this!)
- **fail fast** (i.e. if an incorrect input is sent to your function, stop and display error immediately (best is at compile time, but sometimes you have to do it at runtime))

### Ref

https://www.youtube.com/watch?v=aAb7hSCtvGw&index=1&list=PL0OZE4KoHlVuBT1Z9lL1yAHNclm78GuHy