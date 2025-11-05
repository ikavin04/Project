# Smart Canteen - Complete Code Explanation

## Table of Contents
1. [Project Overview](#project-overview)
2. [File Structure](#file-structure)
3. [HTML Files Explanation](#html-files-explanation)
4. [JavaScript (app.js) Explanation](#javascript-appjs-explanation)
5. [Key Concepts](#key-concepts)

---

## Project Overview

**Smart Canteen** is a web-based canteen management system for KGiSL Institute of Technology. It allows:
- **Students/Users**: Browse menu, add items to cart, place orders, make payments, track orders
- **Admins**: Manage menu items, view/update orders, monitor dashboard statistics

**Technology Stack:**
- Frontend: HTML5, CSS3, Vanilla JavaScript
- Data Storage: Browser's localStorage API (no backend server)
- Design: Responsive mobile-first design

---

## File Structure

```
smart_canteen/
├── index.html                 # Landing page
├── login.html                 # Login page
├── register.html              # Registration page
├── user_dashboard.html        # User menu browsing page
├── payment.html               # Payment processing page
├── order_confirmation.html    # Order success page
├── user_orders.html          # User's order history
├── admin.html                # Admin dashboard
├── app.js                    # All JavaScript functions
├── styles.css                # All styling (explained separately)
└── README.md                 # Project documentation
```

---

## HTML Files Explanation

### 1. **index.html** (Landing Page)

#### Purpose
First page visitors see. Shows features, statistics, and guides users to login/register.

#### Key Sections

```html
<!DOCTYPE html>
```
- **DOCTYPE declaration**: Tells browser this is HTML5 document

```html
<html lang="en">
```
- **lang="en"**: Specifies English language for accessibility

```html
<meta charset="UTF-8">
```
- **charset="UTF-8"**: Supports all characters (including emojis, special symbols)

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```
- **viewport**: Makes website responsive on mobile devices
- **width=device-width**: Match screen width
- **initial-scale=1.0**: Starting zoom level

```html
<link rel="stylesheet" href="styles.css">
```
- **Links external CSS file** for styling

```html
<nav class="navbar">
```
- **Navigation bar**: Top menu with logo and login/register buttons

```html
<section class="hero">
```
- **Hero section**: Main welcome banner with title and call-to-action buttons

```html
<div class="hero-features">
```
- **Feature cards**: Display 4 main features (Browse Menu, Easy Ordering, Quick Payment, Order Tracking)

```html
<section class="about">
```
- **About section**: Describes Smart Canteen and shows statistics

```html
<div class="stats">
```
- **Statistics display**: Shows total orders, users, menu items dynamically

```html
<section class="how-it-works">
```
- **How it works**: 4-step guide explaining the ordering process

```html
<footer class="footer">
```
- **Footer**: Contains links, contact info, copyright notice

#### JavaScript Section

```javascript
const currentUser = getCurrentUser();
```
- **Check if user is logged in**: Retrieves current user from localStorage

```javascript
if (currentUser) {
    if (currentUser.role === 'admin') {
        window.location.href = 'admin.html';
    } else {
        window.location.href = 'user_dashboard.html';
    }
}
```
- **Auto-redirect logic**: If user is already logged in, redirect to appropriate dashboard
- **Admin** → admin.html
- **User** → user_dashboard.html

```javascript
document.addEventListener('DOMContentLoaded', function() {
    updateLandingStats();
});
```
- **DOMContentLoaded event**: Runs code after HTML is fully loaded
- Calls `updateLandingStats()` to populate statistics

```javascript
function updateLandingStats() {
    const orders = getAllOrders();
    const users = getAllUsers();
    const menu = getMenu();
```
- **Retrieves data** from localStorage

```javascript
if (orders.length > 0) {
    document.getElementById('totalOrders').textContent = orders.length + '+';
}
```
- **Updates HTML**: Changes text content of element with id "totalOrders"
- Shows actual number of orders + "+" sign

---

### 2. **login.html** (Login Page)

#### Purpose
Allows users to login with username/password. Includes demo account buttons.

#### Key Elements

```html
<form id="loginForm" class="auth-form">
```
- **Form element**: Collects user input

```html
<input type="text" id="username" name="username" required>
```
- **type="text"**: Text input field
- **id="username"**: Unique identifier for JavaScript access
- **required**: HTML5 validation - field cannot be empty

```html
<input type="password" id="password" name="password" required>
```
- **type="password"**: Masks input text (shows dots/asterisks)

```html
<button type="submit" class="btn btn-primary">Login</button>
```
- **type="submit"**: Triggers form submission when clicked

```html
<div class="demo-accounts">
```
- **Demo buttons**: Allow users to quickly test with demo accounts

#### JavaScript Section

```javascript
initializeDemoAccounts();
```
- **Creates demo accounts** (admin and student1) if they don't exist

```javascript
document.getElementById('loginForm').addEventListener('submit', function(e) {
    e.preventDefault();
```
- **Event listener**: Waits for form submission
- **e.preventDefault()**: Stops page reload (default form behavior)

```javascript
const username = document.getElementById('username').value.trim();
const password = document.getElementById('password').value;
```
- **Gets input values**: Reads what user typed
- **.trim()**: Removes extra spaces from beginning/end

```javascript
if (!username || !password) {
    showMessage('Username and password are required', 'error');
    return;
}
```
- **Validation**: Checks if fields are filled
- **return**: Stops function if validation fails

```javascript
const result = loginUser(username, password);
```
- **Calls login function**: Checks if credentials match any user

```javascript
if (result.success) {
    showMessage('Login successful! Redirecting...', 'success');
    setTimeout(() => {
        if (result.user.role === 'admin') {
            window.location.href = 'admin.html';
        } else {
            window.location.href = 'index.html';
        }
    }, 1000);
}
```
- **Success handling**: Shows success message, waits 1 second, then redirects
- **setTimeout**: Delays execution by 1000 milliseconds (1 second)

```javascript
function fillDemo(type) {
    if (type === 'admin') {
        document.getElementById('username').value = 'admin';
        document.getElementById('password').value = 'admin123';
    }
}
```
- **Auto-fills form**: Puts demo credentials in input fields

---

### 3. **register.html** (Registration Page)

#### Purpose
New users create accounts here.

#### Key Elements

```html
<input type="email" id="email" name="email" required>
```
- **type="email"**: Browser validates email format automatically

```html
<input type="password" id="password" name="password" required minlength="6">
<small>At least 6 characters</small>
```
- **minlength="6"**: HTML5 validation - requires minimum 6 characters
- **<small>**: Helper text explaining requirement

```html
<input type="password" id="confirmPassword" name="confirmPassword" required>
```
- **Confirm password**: User types password twice to avoid typos

#### JavaScript Section

```javascript
if (username.length < 3) {
    showMessage('Username must be at least 3 characters long', 'error');
    return;
}
```
- **Custom validation**: Checks username length in JavaScript

```javascript
if (password !== confirmPassword) {
    showMessage('Passwords do not match', 'error');
    return;
}
```
- **Password matching**: Compares both password fields

```javascript
const result = registerUser(username, email, password);
```
- **Calls registration function**: Creates new user account

```javascript
if (result.success) {
    showMessage('Registration successful! Redirecting to login...', 'success');
    setTimeout(() => {
        window.location.href = 'login.html';
    }, 2000);
}
```
- **Success**: Shows message, waits 2 seconds, redirects to login page

---

### 4. **user_dashboard.html** (Menu Browsing)

#### Purpose
Main page where users browse menu and add items to cart.

#### Key Elements

```html
<button class="hamburger" onclick="toggleMobileMenu()">
    <span></span>
    <span></span>
    <span></span>
</button>
```
- **Hamburger menu**: Three horizontal lines, appears on mobile devices
- **onclick**: Runs function when clicked

```html
<div class="cart-icon" onclick="toggleCart()">
    Cart (<span id="cartCount">0</span>)
</div>
```
- **Cart icon**: Shows number of items in cart
- **<span id="cartCount">**: Dynamic number updated by JavaScript

```html
<select id="categoryFilter" onchange="filterByCategory()">
```
- **Dropdown filter**: Let users filter menu by category
- **onchange**: Runs function when selection changes

```html
<div class="menu-grid" id="menuGrid">
```
- **Menu container**: JavaScript will insert menu items here

```html
<div id="cartPanel" class="cart-panel">
```
- **Sliding cart panel**: Hidden by default, slides in when cart icon clicked

```html
<div id="cartOverlay" class="cart-overlay" onclick="toggleCart()">
```
- **Dark overlay**: Appears behind cart panel, clicking it closes cart

#### JavaScript Section

```javascript
const currentUser = getCurrentUser();
if (!currentUser) {
    window.location.href = 'login.html';
} else if (currentUser.role === 'admin') {
    window.location.href = 'admin.html';
}
```
- **Authentication check**: Ensures user is logged in
- Redirects to login if not logged in
- Redirects admins to admin panel

```javascript
document.addEventListener('DOMContentLoaded', function() {
    document.getElementById('userName').textContent = currentUser.username;
    loadMenu();
    loadCategories();
    updateCartUI();
    startNotificationPolling();
});
```
- **Page initialization**: Runs when page loads
- Sets username display
- Loads menu items
- Populates category filter
- Updates cart display
- Starts checking for order updates

```javascript
function loadMenu() {
    const menu = getMenu();
    const categoryFilter = document.getElementById('categoryFilter').value;
```
- **Gets menu from localStorage**
- **Gets selected category** from dropdown

```javascript
let filteredMenu = menu.filter(item => item.availability);
if (categoryFilter) {
    filteredMenu = filteredMenu.filter(item => item.category === categoryFilter);
}
```
- **Filters menu**: Only shows available items
- If category selected, further filters by category
- **.filter()**: Array method that creates new array with items matching condition

```javascript
menuGrid.innerHTML = filteredMenu.map(item => `
    <div class="menu-item">
        <h3>${item.item_name}</h3>
        <div class="price">₹${item.price.toFixed(2)}</div>
        ...
    </div>
`).join('');
```
- **Creates HTML**: .map() transforms each menu item into HTML string
- **Template literals**: Backticks (`) allow embedded JavaScript ${...}
- **toFixed(2)**: Formats price to 2 decimal places (120.00)
- **.join('')**: Combines array of strings into single string

```javascript
<button onclick="changeQuantity(${item.id}, -1)">-</button>
<span id="qty-${item.id}">0</span>
<button onclick="changeQuantity(${item.id}, 1)">+</button>
```
- **Quantity controls**: Buttons to increase/decrease quantity
- **Dynamic id**: Each item has unique id like "qty-1", "qty-2"

```javascript
let currentQuantities = {};

function changeQuantity(itemId, change) {
    if (!currentQuantities[itemId]) {
        currentQuantities[itemId] = 0;
    }
    currentQuantities[itemId] = Math.max(0, currentQuantities[itemId] + change);
    document.getElementById(`qty-${itemId}`).textContent = currentQuantities[itemId];
}
```
- **Stores quantities**: Object with itemId as key
- **Math.max(0, ...)**: Ensures quantity never goes below 0
- **Updates display**: Changes number shown on page

```javascript
function addToCart(itemId) {
    const quantity = currentQuantities[itemId] || 1;
    const result = addItemToCart(itemId, quantity);
    
    if (result.success) {
        showNotification(`Added ${quantity} item(s) to cart`, 'success');
        updateCartUI();
        currentQuantities[itemId] = 0;
        document.getElementById(`qty-${itemId}`).textContent = '0';
    }
}
```
- **Adds to cart**: Uses quantity from currentQuantities object
- **|| 1**: If no quantity set, defaults to 1
- **Resets quantity**: After adding, quantity display goes back to 0

```javascript
function updateCartUI() {
    const cart = getCart();
    const totalItems = cart.reduce((sum, item) => sum + item.quantity, 0);
    cartCount.textContent = totalItems;
```
- **Updates cart display**: Refreshes cart count, items list, total
- **.reduce()**: Sums up all quantities
  - **sum**: Running total (starts at 0)
  - **item**: Current item being processed

```javascript
if (cart.length === 0) {
    cartItems.innerHTML = '<div class="empty-cart">Your cart is empty</div>';
    checkoutBtn.disabled = true;
}
```
- **Empty cart check**: Shows message if cart empty
- **disabled**: Grays out checkout button

```javascript
cartItems.innerHTML = cart.map(item => `
    <div class="cart-item">
        <h4>${item.name}</h4>
        <button onclick="updateCartQuantity(${item.id}, -1)">-</button>
        <span>${item.quantity}</span>
        <button onclick="updateCartQuantity(${item.id}, 1)">+</button>
        <button onclick="removeFromCart(${item.id})">Remove</button>
    </div>
`).join('');
```
- **Displays cart items**: Creates HTML for each item in cart

```javascript
const total = cart.reduce((sum, item) => sum + (item.price * item.quantity), 0);
cartTotal.textContent = total.toFixed(2);
```
- **Calculates total**: Multiplies price × quantity for each item, sums all

```javascript
function toggleCart() {
    const cartPanel = document.getElementById('cartPanel');
    const cartOverlay = document.getElementById('cartOverlay');
    
    cartPanel.classList.toggle('active');
    cartOverlay.classList.toggle('active');
}
```
- **Opens/closes cart**: Adds/removes "active" class
- CSS uses "active" class to show/hide cart

```javascript
function startNotificationPolling() {
    setInterval(() => {
        checkOrderUpdates();
    }, 30000);
}
```
- **setInterval**: Runs function repeatedly every 30000ms (30 seconds)
- Checks if any orders are ready for pickup

---

### 5. **payment.html** (Payment Processing)

#### Purpose
User reviews order, selects payment method, completes payment.

#### Key Elements

```html
<div class="payment-methods">
    <input type="radio" id="upi" name="paymentMethod" value="UPI" checked>
    <label for="upi">UPI Payment (Demo)</label>
    
    <input type="radio" id="card" name="paymentMethod" value="Card">
    <label for="card">Credit/Debit Card (Demo)</label>
</div>
```
- **Radio buttons**: User can select only one option
- **checked**: UPI is selected by default
- **name="paymentMethod"**: Groups radio buttons together

```html
<div id="upiDetails" class="payment-form">
    <input type="text" id="upiId" placeholder="yourname@upi">
</div>

<div id="cardDetails" class="payment-form hidden">
    <input type="text" id="cardNumber" placeholder="1234 5678 9012 3456">
    <input type="text" id="expiryDate" placeholder="MM/YY">
    <input type="text" id="cvv" placeholder="123">
</div>
```
- **Conditional forms**: Show different fields based on payment method
- **hidden class**: CSS hides element

#### JavaScript Section

```javascript
function loadOrderSummary() {
    const cart = getCart();
    
    if (cart.length === 0) {
        showMessage('Your cart is empty. Redirecting to menu...', 'error');
        setTimeout(() => {
            window.location.href = 'user_dashboard.html';
        }, 2000);
        return;
    }
```
- **Validates cart**: If empty, redirect to menu

```javascript
orderItems.innerHTML = cart.map(item => `
    <div class="order-item">
        <h4>${item.name}</h4>
        <p>₹${item.price.toFixed(2)} × ${item.quantity}</p>
        <div>₹${(item.price * item.quantity).toFixed(2)}</div>
    </div>
`).join('');
```
- **Displays order summary**: Shows each item with price and subtotal

```javascript
function setupPaymentMethodHandlers() {
    const paymentMethods = document.querySelectorAll('input[name="paymentMethod"]');
    
    paymentMethods.forEach(method => {
        method.addEventListener('change', function() {
            if (this.value === 'UPI') {
                upiDetails.classList.remove('hidden');
                cardDetails.classList.add('hidden');
            }
        });
    });
}
```
- **querySelectorAll**: Gets all radio buttons with name="paymentMethod"
- **forEach**: Runs function for each radio button
- **addEventListener('change')**: Runs when radio button selection changes
- **this.value**: Value of clicked radio button

```javascript
function processPayment() {
    const paymentMethod = document.querySelector('input[name="paymentMethod"]:checked').value;
```
- **:checked**: CSS selector for selected radio button

```javascript
if (paymentMethod === 'UPI') {
    const upiId = document.getElementById('upiId').value.trim();
    if (!upiId) {
        showMessage('Please enter your UPI ID', 'error');
        return;
    }
    if (!upiId.includes('@')) {
        showMessage('Please enter a valid UPI ID', 'error');
        return;
    }
}
```
- **Payment validation**: Checks if UPI ID is entered and valid
- **.includes('@')**: Checks if string contains @ symbol

```javascript
showMessage('Processing payment...', 'info');

setTimeout(() => {
    const result = placeOrder(cart, paymentMethod);
    
    if (result.success) {
        showMessage('Payment successful! Order placed.', 'success');
        clearCart();
        
        setTimeout(() => {
            window.location.href = `order_confirmation.html?orderId=${result.orderId}`;
        }, 2000);
    }
}, 2000);
```
- **Simulates payment processing**: Waits 2 seconds (like real payment)
- **placeOrder()**: Saves order to localStorage
- **clearCart()**: Empties cart after successful order
- **?orderId=${result.orderId}**: Passes order ID in URL

```javascript
document.getElementById('cardNumber').addEventListener('input', function() {
    let value = this.value.replace(/\s/g, '');
    let formattedValue = value.replace(/(.{4})/g, '$1 ').trim();
    this.value = formattedValue;
});
```
- **Auto-formatting**: Adds space after every 4 digits
- **/\s/g**: Regex that finds all spaces
- **replace(/\s/g, '')**: Removes all spaces
- **/(.{4})/g**: Regex that matches groups of 4 characters
- **'$1 '**: Adds space after each group

```javascript
document.getElementById('expiryDate').addEventListener('input', function() {
    let value = this.value.replace(/\D/g, '');
    if (value.length >= 2) {
        value = value.substring(0, 2) + '/' + value.substring(2, 4);
    }
    this.value = value;
});
```
- **/\D/g**: Regex for non-digit characters
- **Formats as MM/YY**: Automatically adds slash after 2 digits

---

### 6. **order_confirmation.html** (Success Page)

#### Purpose
Shows order details after successful payment.

#### Key Elements

```html
<div class="success-icon">✓</div>
```
- **Checkmark symbol**: Visual confirmation of success

```html
<div class="order-details" id="orderDetails">
```
- **Order details container**: JavaScript will populate this

#### JavaScript Section

```javascript
const urlParams = new URLSearchParams(window.location.search);
const orderId = urlParams.get('orderId');
```
- **URLSearchParams**: Reads parameters from URL
- **Example**: If URL is `...?orderId=ORD-ABC123`
- **orderId** will be `ORD-ABC123`

```javascript
const orders = getUserOrders();
const order = orders.find(o => o.order_id === orderId);
```
- **.find()**: Finds first item matching condition

```javascript
let html = '<div class="order-summary-section">';
html += '<h3 class="section-title">Order Summary</h3>';
html += '<div class="order-info-box">';

html += '<div class="info-item">';
html += '<div class="info-label">Order ID</div>';
html += `<div class="info-value">${order.order_id}</div>`;
html += '</div>';
```
- **Building HTML string**: Manually constructs HTML
- **+=**: Adds to existing string

```javascript
html += '<div class="info-item">';
html += '<div class="info-label">Order Time</div>';
html += `<div class="info-value">${new Date(order.timestamp).toLocaleString()}</div>`;
html += '</div>';
```
- **new Date()**: Creates date object from timestamp
- **toLocaleString()**: Converts to readable date/time format
- **Example**: "11/5/2025, 10:37:17 AM"

```javascript
order.items.forEach(item => {
    html += '<div class="item-card">';
    html += `<div class="item-name">${item.name}</div>`;
    html += `<div class="item-qty">Quantity: ${item.quantity}</div>`;
    html += `<div class="item-price">₹${(item.price * item.quantity).toFixed(2)}</div>`;
    html += '</div>';
});
```
- **forEach()**: Loops through each item in order

```javascript
document.getElementById('orderDetails').innerHTML = html;
```
- **Sets innerHTML**: Replaces content of element with constructed HTML

---

### 7. **user_orders.html** (Order History)

#### Purpose
Shows all user's past and current orders.

#### Key Elements

```html
<select id="statusFilter" onchange="filterOrders()">
    <option value="">All Orders</option>
    <option value="Uncompleted">Preparing</option>
    <option value="Ready">Ready for Pickup</option>
    <option value="Completed">Completed</option>
</select>
```
- **Status filter**: User can filter orders by status

```html
<div id="orderModal" class="modal">
```
- **Modal window**: Popup that shows order details

#### JavaScript Section

```javascript
filteredOrders.sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
```
- **.sort()**: Arranges array in order
- **Subtracts dates**: Newer dates have bigger numbers
- **Result**: Newest orders first

```javascript
if (filteredOrders.length === 0) {
    ordersContainer.innerHTML = `
        <div class="no-orders">
            <p>No orders found.</p>
            <a href="user_dashboard.html">Order Now</a>
        </div>
    `;
    return;
}
```
- **Empty state**: Shows message if no orders found

```javascript
ordersContainer.innerHTML = filteredOrders.map(order => `
    <div class="order-card">
        <div class="order-status status-${order.status.toLowerCase()}">
            ${order.status}
        </div>
        ...
    </div>
`).join('');
```
- **Displays order cards**: Creates card for each order
- **status-${order.status.toLowerCase()}**: Dynamic CSS class
- **Example**: "status-uncompleted", "status-ready"

```javascript
${order.items.slice(0, 2).map(item => item.name).join(', ')}
${order.items.length > 2 ? '...' : ''}
```
- **.slice(0, 2)**: Gets first 2 items from array
- **.map(item => item.name)**: Extracts just names
- **.join(', ')**: Combines with comma separator
- **Ternary operator**: condition ? ifTrue : ifFalse
- **Result**: "Chicken Burger, Coffee ..."

```javascript
function downloadBill(orderId) {
    const order = orders.find(o => o.order_id === orderId);
    
    if (order.status !== 'Ready' && order.status !== 'Completed') {
        showNotification('Bill not available yet...', 'warning');
        return;
    }
    
    generateBillPDF(order);
}
```
- **Bill availability**: Only ready/completed orders can download bill

```javascript
function generateBillPDF(order) {
    const billContent = `
        <div style="font-family: Arial; padding: 20px;">
            <h1>KGiSL Institute of Technology</h1>
            <h2>Smart Canteen - Order Bill</h2>
            <table>
                <tr><td>Order ID:</td><td>${order.order_id}</td></tr>
                ...
            </table>
        </div>
    `;
    
    const printWindow = window.open('', '_blank');
    printWindow.document.write(billContent);
    printWindow.document.close();
}
```
- **Creates new window**: Opens bill in separate window
- **Inline styles**: CSS written directly in HTML
- **window.open('', '_blank')**: Opens new browser tab
- **.document.write()**: Writes HTML to new window

```javascript
function startOrderStatusPolling() {
    setInterval(() => {
        currentOrders.forEach(currentOrder => {
            const previousOrder = previousOrders.find(p => p.order_id === currentOrder.order_id);
            if (previousOrder && previousOrder.status !== currentOrder.status) {
                showNotification(`Order ${currentOrder.order_id} is ready!`, 'success');
                loadOrders();
            }
        });
    }, 30000);
}
```
- **Checks for updates**: Every 30 seconds
- **Compares old vs new**: Detects status changes
- **Shows notification**: Alerts user when order ready

```javascript
window.onclick = function(event) {
    const modal = document.getElementById('orderModal');
    if (event.target === modal) {
        closeOrderModal();
    }
}
```
- **Click outside to close**: Closes modal when clicking backdrop
- **event.target**: Element that was clicked

---

### 8. **admin.html** (Admin Dashboard)

#### Purpose
Admin panel to manage menu items, orders, and view statistics.

#### Key Elements

```html
<div class="admin-tabs">
    <button class="tab-button active" onclick="showTab('dashboard')">Dashboard</button>
    <button class="tab-button" onclick="showTab('menu')">Menu Management</button>
    <button class="tab-button" onclick="showTab('orders')">Order Management</button>
</div>
```
- **Tabs**: Switch between different admin sections

```html
<div id="dashboard" class="tab-content active">
```
- **Tab content**: Only one tab visible at a time

#### JavaScript Section

```javascript
if (currentUser.role !== 'admin') {
    window.location.href = 'user_dashboard.html';
}
```
- **Admin-only access**: Redirects non-admins

```javascript
function showTab(tabName) {
    const tabContents = document.querySelectorAll('.tab-content');
    tabContents.forEach(tab => tab.classList.remove('active'));
    
    const tabButtons = document.querySelectorAll('.tab-button');
    tabButtons.forEach(button => button.classList.remove('active'));
    
    document.getElementById(tabName).classList.add('active');
    event.target.classList.add('active');
}
```
- **Tab switching**: Hides all tabs, shows selected one

```javascript
function loadDashboardStats() {
    const users = getAllUsers();
    const orders = getAllOrders();
    const menu = getMenu();
    
    const totalUsers = users.filter(user => user.role === 'user').length;
    const totalRevenue = orders.reduce((sum, order) => sum + order.total_amount, 0);
```
- **Calculates statistics**: Counts users, sums revenue

```javascript
const today = new Date().toDateString();
const ordersToday = orders.filter(order => 
    new Date(order.timestamp).toDateString() === today
);
```
- **Filters today's orders**: Compares date strings

```javascript
menuTableBody.innerHTML = menu.map(item => `
    <tr>
        <td>${item.id}</td>
        <td>${item.item_name}</td>
        <td>
            <button onclick="editMenuItem(${item.id})">Edit</button>
            <button onclick="deleteMenuItem(${item.id})">Delete</button>
        </td>
    </tr>
`).join('');
```
- **Displays menu table**: Creates row for each menu item

```javascript
function editMenuItem(itemId) {
    const item = menu.find(i => i.id === itemId);
    
    document.getElementById('editItemId').value = itemId;
    document.getElementById('itemName').value = item.item_name;
    document.getElementById('itemPrice').value = item.price;
    
    document.getElementById('itemFormContainer').classList.remove('hidden');
}
```
- **Edits item**: Fills form with existing item data

```javascript
function deleteMenuItem(itemId) {
    if (confirm('Are you sure you want to delete this menu item?')) {
        const result = deleteMenuItemById(itemId);
        ...
    }
}
```
- **confirm()**: Shows browser dialog with OK/Cancel buttons
- Returns true if user clicks OK, false if Cancel

```javascript
ordersTableBody.innerHTML = filteredOrders.map(order => `
    <td>
        <select onchange="updateOrderStatus('${order.order_id}', this.value)">
            <option value="Uncompleted" ${order.status === 'Uncompleted' ? 'selected' : ''}>Uncompleted</option>
            <option value="Ready" ${order.status === 'Ready' ? 'selected' : ''}>Ready</option>
            <option value="Completed" ${order.status === 'Completed' ? 'selected' : ''}>Completed</option>
        </select>
    </td>
`).join('');
```
- **Status dropdown**: Admin can change order status
- **${...? 'selected' : ''}**: Marks current status as selected

```javascript
document.getElementById('menuItemForm').addEventListener('submit', function(e) {
    e.preventDefault();
    
    const itemId = document.getElementById('editItemId').value;
    const itemData = {
        item_name: document.getElementById('itemName').value.trim(),
        price: parseFloat(document.getElementById('itemPrice').value),
        ...
    };
    
    let result;
    if (itemId) {
        result = updateMenuItem(parseInt(itemId), itemData);
    } else {
        result = addMenuItem(itemData);
    }
}
```
- **Form submission**: Handles both add and edit
- **parseFloat()**: Converts string to decimal number
- **parseInt()**: Converts string to integer

---

## JavaScript (app.js) Explanation

### Data Initialization Functions

```javascript
function initializeData() {
    if (!localStorage.getItem('smartCanteenMenu')) {
        const sampleMenu = [
            {
                id: 1,
                item_name: 'Chicken Burger',
                price: 120.00,
                category: 'Main Course',
                availability: true
            },
            ...
        ];
        localStorage.setItem('smartCanteenMenu', JSON.stringify(sampleMenu));
    }
}
```
**Purpose**: Creates sample menu data on first use
- **localStorage.getItem()**: Reads from browser storage
- **!**: NOT operator, checks if data doesn't exist
- **JSON.stringify()**: Converts JavaScript object to string (required for localStorage)
- **localStorage.setItem()**: Saves to browser storage

### Authentication Functions

```javascript
function registerUser(username, email, password) {
    const users = getAllUsers();
    
    if (users.find(user => user.username === username)) {
        return { success: false, message: 'Username already exists' };
    }
    
    const newUser = {
        id: Math.max(...users.map(u => u.id), 0) + 1,
        username,
        email,
        password,
        role: 'user',
        created_at: new Date().toISOString()
    };
    
    users.push(newUser);
    localStorage.setItem('smartCanteenUsers', JSON.stringify(users));
    
    return { success: true, message: 'Registration successful' };
}
```
**Line-by-line breakdown**:
1. **const users = getAllUsers()**: Gets existing users
2. **users.find()**: Searches for duplicate username
3. **return { success: false, ... }**: Returns object with result
4. **Math.max()**: Finds highest user ID
5. **...users.map(u => u.id)**: Spread operator, expands array
6. **+ 1**: Increments to get new ID
7. **username**: Shorthand for username: username
8. **users.push()**: Adds new user to array
9. **JSON.stringify()**: Converts to string for storage

```javascript
function loginUser(username, password) {
    const users = getAllUsers();
    const user = users.find(u => u.username === username && u.password === password);
    
    if (!user) {
        return { success: false, message: 'Invalid username or password' };
    }
    
    localStorage.setItem('smartCanteenCurrentUser', JSON.stringify(user));
    return { success: true, message: 'Login successful', user };
}
```
**Purpose**: Validates credentials and logs in user
- **&&**: AND operator, both conditions must be true
- Stores logged-in user in 'smartCanteenCurrentUser'

```javascript
function logout() {
    localStorage.removeItem('smartCanteenCurrentUser');
    localStorage.removeItem('smartCanteenCart');
    window.location.href = 'login.html';
}
```
**Purpose**: Logs out user, clears data, redirects to login

```javascript
function getCurrentUser() {
    const currentUser = localStorage.getItem('smartCanteenCurrentUser');
    return currentUser ? JSON.parse(currentUser) : null;
}
```
**Purpose**: Gets currently logged-in user
- **JSON.parse()**: Converts string back to JavaScript object
- **? ... : ...**: Ternary operator (if-else shorthand)
- Returns null if no user logged in

### Menu Management Functions

```javascript
function getMenu() {
    return JSON.parse(localStorage.getItem('smartCanteenMenu') || '[]');
}
```
**Purpose**: Retrieves menu from localStorage
- **|| '[]'**: Default to empty array if null

```javascript
function addMenuItem(itemData) {
    const menu = getMenu();
    const newId = Math.max(...menu.map(item => item.id), 0) + 1;
    
    const newItem = {
        id: newId,
        item_name: itemData.item_name,
        price: parseFloat(itemData.price),
        category: itemData.category,
        availability: itemData.availability !== false
    };
    
    menu.push(newItem);
    saveMenu(menu);
    
    return { success: true, message: 'Menu item added successfully' };
}
```
**Purpose**: Adds new menu item
- **parseFloat()**: Ensures price is a number
- **!== false**: Defaults to true if not specified

```javascript
function toggleMenuItemAvailability(id) {
    const menu = getMenu();
    const item = menu.find(item => item.id === id);
    
    if (!item) {
        return { success: false, message: 'Item not found' };
    }
    
    item.availability = !item.availability;
    saveMenu(menu);
    
    return { success: true, message: 'Availability updated successfully' };
}
```
**Purpose**: Toggles availability on/off
- **!item.availability**: NOT operator, flips boolean

### Cart Functions

```javascript
function addItemToCart(itemId, quantity = 1) {
    const menuItem = getMenuItemById(itemId);
    
    if (!menuItem) {
        return { success: false, message: 'Item not found' };
    }
    
    if (!menuItem.availability) {
        return { success: false, message: 'Item is not available' };
    }
    
    const cart = getCart();
    const existingItem = cart.find(item => item.id === itemId);
    
    if (existingItem) {
        existingItem.quantity += quantity;
    } else {
        cart.push({
            id: menuItem.id,
            name: menuItem.item_name,
            price: menuItem.price,
            quantity: quantity
        });
    }
    
    saveCart(cart);
    return { success: true, message: 'Item added to cart' };
}
```
**Purpose**: Adds item to cart
- **quantity = 1**: Default parameter
- Checks if item already in cart
- If yes: increases quantity
- If no: adds new cart item

```javascript
function updateItemQuantity(itemId, change) {
    const cart = getCart();
    const item = cart.find(item => item.id === itemId);
    
    item.quantity += change;
    
    if (item.quantity <= 0) {
        return removeItemFromCart(itemId);
    }
    
    saveCart(cart);
    return { success: true, message: 'Quantity updated' };
}
```
**Purpose**: Updates item quantity
- **change**: Can be +1 or -1
- If quantity reaches 0, removes item

### Order Functions

```javascript
function generateOrderId() {
    return 'ORD-' + Math.random().toString(36).substr(2, 6).toUpperCase();
}
```
**Purpose**: Creates unique order ID
- **Math.random()**: Random number between 0 and 1
- **.toString(36)**: Converts to base-36 (0-9, a-z)
- **.substr(2, 6)**: Takes 6 characters starting at position 2
- **.toUpperCase()**: Converts to uppercase
- **Result**: "ORD-H21VYI"

```javascript
function placeOrder(cartItems, paymentMethod = 'UPI') {
    const currentUser = getCurrentUser();
    const orders = getAllOrders();
    const orderId = generateOrderId();
    const transactionId = generateTransactionId();
    
    const totalAmount = cartItems.reduce((sum, item) => 
        sum + (item.price * item.quantity), 0
    );
    
    const newOrder = {
        id: orders.length + 1,
        order_id: orderId,
        user_id: currentUser.id,
        items: cartItems.map(item => ({
            id: item.id,
            name: item.name,
            price: item.price,
            quantity: item.quantity
        })),
        total_amount: totalAmount,
        status: 'Uncompleted',
        payment_method: paymentMethod,
        payment_status: 'Paid',
        transaction_id: transactionId,
        timestamp: new Date().toISOString()
    };
    
    orders.push(newOrder);
    saveOrders(orders);
    
    return { 
        success: true, 
        orderId: orderId,
        transactionId: transactionId,
        totalAmount: totalAmount
    };
}
```
**Purpose**: Creates new order
- Generates unique IDs
- Calculates total
- Saves order with all details
- Returns order information

```javascript
function updateOrderStatusById(orderId, newStatus) {
    const orders = getAllOrders();
    const order = orders.find(order => order.order_id === orderId);
    
    order.status = newStatus;
    
    if (newStatus === 'Ready') {
        order.ready_at = new Date().toISOString();
    } else if (newStatus === 'Completed') {
        order.completed_at = new Date().toISOString();
    }
    
    saveOrders(orders);
    return { success: true, message: 'Order status updated successfully' };
}
```
**Purpose**: Admin updates order status
- Timestamps when order becomes ready/completed

### Utility Functions

```javascript
function initializeMobileNav() {
    const hamburger = document.querySelector('.hamburger');
    const navLinks = document.querySelector('.nav-links');
    
    if (hamburger && navLinks) {
        hamburger.addEventListener('click', () => {
            hamburger.classList.toggle('active');
            navLinks.classList.toggle('active');
        });
        
        document.addEventListener('click', (e) => {
            if (!hamburger.contains(e.target) && !navLinks.contains(e.target)) {
                hamburger.classList.remove('active');
                navLinks.classList.remove('active');
            }
        });
    }
}
```
**Purpose**: Mobile menu functionality
- Opens/closes menu on hamburger click
- Closes menu when clicking outside
- **.contains()**: Checks if element contains another element

```javascript
document.addEventListener('DOMContentLoaded', () => {
    initializeMobileNav();
});
```
**Purpose**: Runs initialization when page loads

```javascript
initializeData();
initializeDemoAccounts();
```
**Purpose**: Runs immediately when app.js loads
- Creates sample data if needed

---

## Key Concepts

### 1. **localStorage**
Browser feature that stores data permanently (until cleared).

```javascript
// Save data
localStorage.setItem('key', 'value');

// Get data
const value = localStorage.getItem('key');

// Remove data
localStorage.removeItem('key');
```

**Important**: localStorage only stores strings
- Use `JSON.stringify()` to save objects/arrays
- Use `JSON.parse()` to read them back

### 2. **Event Listeners**
Wait for user actions and respond.

```javascript
element.addEventListener('click', function() {
    // Runs when element is clicked
});

element.addEventListener('submit', function(e) {
    e.preventDefault(); // Stops default behavior
    // Handle form submission
});
```

### 3. **DOM Manipulation**
JavaScript interacting with HTML.

```javascript
// Get element
const element = document.getElementById('myElement');

// Change content
element.textContent = 'New text';
element.innerHTML = '<strong>Bold text</strong>';

// Change classes
element.classList.add('active');
element.classList.remove('hidden');
element.classList.toggle('open');
```

### 4. **Array Methods**

```javascript
// map: Transform each item
const numbers = [1, 2, 3];
const doubled = numbers.map(n => n * 2); // [2, 4, 6]

// filter: Keep items matching condition
const numbers = [1, 2, 3, 4];
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// find: Get first item matching condition
const users = [{name: 'John'}, {name: 'Jane'}];
const jane = users.find(u => u.name === 'Jane'); // {name: 'Jane'}

// reduce: Combine all items into single value
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((total, n) => total + n, 0); // 10
```

### 5. **Arrow Functions**
Shorter function syntax.

```javascript
// Traditional function
function add(a, b) {
    return a + b;
}

// Arrow function
const add = (a, b) => a + b;

// With single parameter (parentheses optional)
const double = n => n * 2;

// With multiple lines (need curly braces and return)
const add = (a, b) => {
    const result = a + b;
    return result;
};
```

### 6. **Template Literals**
Strings with embedded expressions.

```javascript
const name = 'John';
const age = 25;

// Old way
const message = 'My name is ' + name + ' and I am ' + age;

// Template literal
const message = `My name is ${name} and I am ${age}`;

// Multi-line
const html = `
    <div>
        <h1>${name}</h1>
        <p>Age: ${age}</p>
    </div>
`;
```

### 7. **Ternary Operator**
Short if-else statement.

```javascript
// Traditional if-else
let status;
if (age >= 18) {
    status = 'adult';
} else {
    status = 'minor';
}

// Ternary operator
const status = age >= 18 ? 'adult' : 'minor';
```

### 8. **Object Destructuring & Spread**

```javascript
// Destructuring
const user = { name: 'John', age: 25 };
const { name, age } = user;

// Spread operator
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = [...arr1, ...arr2]; // [1, 2, 3, 4]

const obj1 = { a: 1 };
const obj2 = { b: 2 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 2 }
```

---

## Data Flow Example: Placing an Order

1. **User adds item to cart** (user_dashboard.html)
   ```javascript
   addToCart(itemId) → addItemToCart() → saveCart()
   ```

2. **Cart saved to localStorage**
   ```javascript
   localStorage.setItem('smartCanteenCart', JSON.stringify(cart))
   ```

3. **User proceeds to payment** (payment.html)
   ```javascript
   loadOrderSummary() → getCart() → displays items
   ```

4. **User completes payment**
   ```javascript
   processPayment() → placeOrder() → generateOrderId() → saveOrders()
   ```

5. **Order saved to localStorage**
   ```javascript
   localStorage.setItem('smartCanteenOrders', JSON.stringify(orders))
   ```

6. **User redirected to confirmation**
   ```javascript
   window.location.href = 'order_confirmation.html?orderId=...'
   ```

7. **Confirmation page loads**
   ```javascript
   loadOrderDetails() → getUserOrders() → displays order
   ```

8. **Admin sees new order** (admin.html)
   ```javascript
   loadOrders() → getAllOrders() → displays in table
   ```

9. **Admin updates status**
   ```javascript
   updateOrderStatus() → updateOrderStatusById() → saveOrders()
   ```

10. **User notified** (user_orders.html)
    ```javascript
    startOrderStatusPolling() → checks every 30s → showNotification()
    ```

---

## Security Notes

**Important**: This is a demo application. In production:

1. **Never store passwords in plain text**
   - Use proper backend authentication
   - Hash passwords with bcrypt

2. **Don't use localStorage for sensitive data**
   - Use secure HTTP-only cookies
   - Implement JWT tokens

3. **Validate on server-side**
   - All validation happens in browser (can be bypassed)
   - Need backend server for security

4. **Use HTTPS**
   - Encrypt data transmission

5. **Implement proper payment gateway**
   - This is demo only
   - Use Razorpay, Stripe, etc. in production

---

## Common JavaScript Patterns Used

### 1. **IIFE (Immediately Invoked Function Expression)**
```javascript
(function() {
    // Runs immediately
    initializeData();
})();
```

### 2. **Callback Functions**
```javascript
setTimeout(() => {
    console.log('Runs after 1 second');
}, 1000);
```

### 3. **Error Handling**
```javascript
if (!user) {
    return { success: false, message: 'Not found' };
}
```

### 4. **Default Parameters**
```javascript
function addItem(id, quantity = 1) {
    // quantity is 1 if not provided
}
```

### 5. **Short-circuit Evaluation**
```javascript
const value = getValue() || 'default';
const name = user && user.name;
```

---

This explanation covers every major aspect of your Smart Canteen application. Each function, HTML element, and JavaScript concept has been explained in detail with examples.
