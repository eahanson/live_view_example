# LiveViewExample

**scroll-to-anchor** branch

Exposes an issue with scrolling to an anchor on page load. 

I would expect that visting [http://localhost:4000/#300](http://localhost:4000/#300), 
which is a LiveView, would scroll to the div with id "300", but it does not.

Visting [http://localhost:4000/pages#300](http://localhost:4000/pages#300),
which is not a LiveView, does scroll to the div with id "300".

Visiting [http://localhost:4000/?js-fix#300](http://localhost:4000/?js-fix#300),
which is a LiveView with the following JS added, fixes the issue:

```javascript
window.addEventListener("phx:page-loading-stop", (info) => {
  console.log("window.location", window.location);
  if (window.location.search === "?js-fix") {
    let hashEl = Browser.getHashTargetEl(window.location.hash);
    if (hashEl) {
      hashEl.scrollIntoView();
    }
  }
});
```
