---
title: Introduction to ChildNode
---
ChildNode in the DEMO-one-app repository refers to a JavaScript interface that contains methods that are particular to Node objects that can have a parent. The most commonly used method in this interface is the 'remove' method. This method is used to remove the object from the tree it belongs to.

<SwmSnippet path="/src/client/polyfill/ChildNode.remove.js" line="23">

---

## Usage of `remove` in the codebase

Here, a polyfill is created for the `remove` method. This is done to ensure compatibility with browsers that do not support the `remove` method natively.

```javascript
  Element.prototype.remove = function remove() {
    if (this.parentNode) {
      this.remove();
    }
  };
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/initClient.jsx" line="77">

---

In this file, the `remove` method is used to remove style elements and a script element from the DOM. This is done to clean up the DOM after the initial render.

```javascript
    [...document.querySelectorAll('.ssr-css')]
      .forEach((style) => style.remove());

    delete global.__INITIAL_STATE__;
    document.querySelector('#initial-state').remove();
  } catch (error) {
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/prerender.js" line="51">

---

Here, the `remove` method is used to remove script elements from the body of the document. This is done as part of the prerendering process.

```javascript
    bodyHelmetScripts.forEach((script) => script.remove());
    // If the helmet scripts exist in the head then we can assume that the body
```

---

</SwmSnippet>

<SwmSnippet path="/prod-sample/sample-modules/needy-frank/0.0.0/src/iguazu.js" line="55">

---

In this file, the `remove` method is used to remove items from a cache. This is done when the result of a function call is undefined.

```javascript
        typeof result === 'undefined' && typeof error === 'undefined'
          ? cache.remove(hash(args))
          : cache.set(hash(args), error || result)
```

---

</SwmSnippet>

# ChildNode functions

The remove function of ChildNode

<SwmSnippet path="/src/client/polyfill/ChildNode.remove.js" line="23">

---

## The remove function

The 'remove' function is a method that removes the object from the tree it belongs to. If the object has a parent, this function causes the object to be removed from the child nodes of its parent.

```javascript
  Element.prototype.remove = function remove() {
    if (this.parentNode) {
      this.remove();
    }
  };
```

---

</SwmSnippet>

<SwmSnippet path="/src/client/initClient.jsx" line="77">

---

## Usage of remove function

The 'remove' function is used in the initClient file to remove elements from the DOM. It is used to remove style elements and the initial state element from the document.

```javascript
    [...document.querySelectorAll('.ssr-css')]
      .forEach((style) => style.remove());

    delete global.__INITIAL_STATE__;
    document.querySelector('#initial-state').remove();
  } catch (error) {
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1vbmUtYXBwJTNBJTNBZ2lsYWRuYXZvdA==" repo-name="DEMO-one-app" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>
