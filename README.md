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

# 🧩 Pure JavaScript Version (Minified / Bookmarklet)

Here’s a **clean minified version** you can include:

```javascript
(function(){console.clear();const t=1000,s=512000,r=performance.getEntriesByType("resource");let o=[],l=[],a=[];r.forEach(e=>{const n=e.duration,c=e.transferSize||0;n>t&&o.push({name:e.name,time:n.toFixed(2)+" ms",type:e.initiatorType}),c>s&&l.push({name:e.name,size:(c/1024).toFixed(2)+" KB",type:e.initiatorType}),("xmlhttprequest"===e.initiatorType||"fetch"===e.initiatorType)&&a.push({name:e.name,time:n.toFixed(2)+" ms"})}),o.sort((e,t)=>parseFloat(t.time)-parseFloat(e.time)),console.log("🐢 Slow Requests"),console.table(o.slice(0,10)),console.log("📦 Large Resources"),console.table(l.slice(0,10)),console.log("🔗 API Calls"),console.table(a.slice(0,10));})();
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
