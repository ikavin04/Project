KGiSL Smart Canteen — Frontend-only version

This is a beginner-friendly, static frontend version of the Smart Canteen app. It simulates a backend using the browser's localStorage. No server is required — open `index.html` in a browser to try it.

Files:
- index.html — public menu and cart
- admin.html — simple admin panel to add menu items, toggle availability, and update orders
- styles.css — simple responsive styles
- app.js — main JavaScript, uses localStorage to persist menu, cart and orders

How to run:
1. Open `frontend_static/index.html` in any modern browser (Chrome, Edge, Firefox).
2. Browse the menu, add items to the cart, and click "Place Order".
3. Open `frontend_static/admin.html` to see orders and mark them "Ready" or "Completed" or to add/toggle menu items.

Key concepts (easy to explain):
- Data persistence: localStorage stores JSON strings (menu, cart, orders).
- Rendering: JavaScript reads stored data and builds HTML elements dynamically.
- Event handlers: Add to cart, place order, toggle availability are plain DOM events.
- No backend: Everything runs locally in the browser — ideal to learn frontend basics.

Notes:
- This simplified app replaces server-side logic with localStorage for learning/demonstration.
- For production you would replace storage with HTTP calls to a backend (Flask / Node / etc.).

If you want, I can also:
- Add an `orders.html` user page to view placed orders (already supported by app.js if you add the element).
- Add printable bill generation using `window.print()` or a lightweight library.
- Convert this static frontend to integrate with your Flask backend later.