# 🚀 Web Performance Console Debugger

A lightweight **copy-paste JavaScript snippet** to quickly analyze website performance directly from the browser console.

No setup. No installation. Just paste and debug.

---

## 🔥 Features

- 🐢 Detect slow network requests
- 📦 Identify large resources
- 🔗 List API calls (XHR / Fetch)
- 🧱 Find heavy DOM elements
- ⏱ Show page load timing (TTFB, DOM load, total load)
- 🔥 Highlight likely performance bottleneck

---

## ⚡ How to Use

1. Open any website
2. Open DevTools (`F12`)
3. Go to **Console**
4. Paste the script
5. Press Enter

---

## 📌 Script

```javascript
(function () { console.clear(); console.log("🚀 Enhanced Debugger Started...\n"); const slowThreshold = 1000; const largeSizeThreshold = 500 * 1024; const resources = performance.getEntriesByType("resource"); let slowRequests = []; let largeResources = []; let apiCalls = []; resources.forEach((res) => { const time = res.duration; const size = res.transferSize || 0; if (time > slowThreshold) { slowRequests.push({ name: res.name, time: time.toFixed(2) + " ms", type: res.initiatorType, }); } if (size > largeSizeThreshold) { largeResources.push({ name: res.name, size: (size / 1024).toFixed(2) + " KB", type: res.initiatorType, }); } if (res.initiatorType === "xmlhttprequest" || res.initiatorType === "fetch") { apiCalls.push({ name: res.name, time: time.toFixed(2) + " ms", }); } }); slowRequests.sort((a, b) => parseFloat(b.time) - parseFloat(a.time)); console.log("🐢 Slow Requests:"); console.table(slowRequests.slice(0, 10)); console.log("📦 Large Resources:"); console.table(largeResources.slice(0, 10)); console.log("🔗 API Calls:"); console.table(apiCalls.slice(0, 10)); // ========================= // 🧱 DOM ANALYSIS // ========================= const all = document.getElementsByTagName("*"); let heavyNodes = []; for (let el of all) { const children = el.children.length; const size = (el.innerHTML || "").length; if (children > 50 || size > 5000) { heavyNodes.push({ tag: el.tagName, id: el.id, class: el.className, children, htmlSize: size, }); } } heavyNodes.sort((a, b) => b.htmlSize - a.htmlSize); console.log("🧱 Heavy DOM Nodes:"); console.table(heavyNodes.slice(0, 10)); console.log("📊 Total DOM Elements:", all.length); // ========================= // ⏱ NAVIGATION TIMING // ========================= const nav = performance.getEntriesByType("navigation")[0]; if (nav) { console.log("⏱ Page Load Breakdown:"); console.table({ "TTFB (Backend)": (nav.responseStart - nav.requestStart).toFixed(2) + " ms", "DOM Load": (nav.domContentLoadedEventEnd - nav.startTime).toFixed(2) + " ms", "Total Load": (nav.loadEventEnd - nav.startTime).toFixed(2) + " ms", }); } // ========================= // 🔥 BEST GUESS // ========================= if (slowRequests.length > 0) { console.log("🔥 Likely Issue:"); console.log(slowRequests[0]); } console.log("\n✅ Done"); })();
```


---

# 🧩 Pure JavaScript Version (Minified / Bookmarklet)

Here’s a **clean minified version** you can include:

```javascript
javascript:(function(){console.clear();const t=1000,s=512000,r=performance.getEntriesByType("resource");let o=[],l=[],a=[];r.forEach(e=>{const n=e.duration,c=e.transferSize||0;n>t&&o.push({name:e.name,time:n.toFixed(2)+" ms",type:e.initiatorType}),c>s&&l.push({name:e.name,size:(c/1024).toFixed(2)+" KB",type:e.initiatorType}),("xmlhttprequest"===e.initiatorType||"fetch"===e.initiatorType)&&a.push({name:e.name,time:n.toFixed(2)+" ms"})}),o.sort((e,t)=>parseFloat(t.time)-parseFloat(e.time)),console.log("🐢 Slow Requests"),console.table(o.slice(0,10)),console.log("📦 Large Resources"),console.table(l.slice(0,10)),console.log("🔗 API Calls"),console.table(a.slice(0,10));})();
```
# 🧠 Example Output
- Slow API calls (>1s)
- Large JS/CSS files (>500KB)
- Heavy DOM nodes (large HTML or too many children)
- Backend response time (TTFB)

# ⚠️ Limitations
- Cannot access request payload (browser security restriction)
- Works on already loaded resources (unless used before reload)
- Some frameworks may hide API calls

# 🔥 Pro Tips
- Run performance.clearResourceTimings() before actions (like login)
- Then trigger action and re-run script
- Use alongside Chrome DevTools Network tab for deeper debugging
