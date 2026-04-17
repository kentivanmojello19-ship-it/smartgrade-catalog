# smartgrade-catalog
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SmartGrade | Advanced Order Manager</title>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@700;800&family=Playfair+Display:ital,wght@0,900;1,900&family=Poppins:wght@400;600&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        :root {
            --gold: linear-gradient(to right, #bf953f, #fcf6ba, #b38728, #fcf6ba, #aa771c);
            --premium-bg: #0f172a;
            --accent: #ef4444;
            --success: #10b981;
            --card-shadow: 0 10px 20px rgba(0,0,0,0.2);
        }

        * { margin: 0; padding: 0; box-sizing: border-box; font-family: 'Poppins', sans-serif; }
        body { background: var(--premium-bg); color: #fff; padding: 20px; }

        /* Dashboard Area */
        .dashboard { max-width: 1200px; margin: 0 auto 30px; background: rgba(255,255,255,0.05); padding: 25px; border-radius: 20px; border: 1px solid rgba(255,255,255,0.1); box-shadow: var(--card-shadow); }
        .admin-row { display: flex; gap: 10px; flex-wrap: wrap; margin-top: 15px; }
        .admin-row input { flex: 1; padding: 12px; border-radius: 8px; border: none; min-width: 150px; outline: none; }
        
        button { cursor: pointer; border: none; border-radius: 8px; font-weight: bold; transition: 0.3s; }
        .btn-add { background: var(--success); color: white; padding: 10px 20px; }
        .btn-dl { background: #f59e0b; color: #000; padding: 10px 20px; }

        /* Main Layout */
        .main-container { display: grid; grid-template-columns: 1.8fr 1.2fr; gap: 25px; max-width: 1300px; margin: 0 auto; }

        /* Catalog Section */
        .catalog-section { background: #f1f5f9; border-radius: 20px; padding: 25px; color: #333; height: fit-content; }
        .grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(240px, 1fr)); gap: 20px; }
        .card { background: white; border-radius: 15px; padding: 15px; text-align: center; box-shadow: 0 5px 15px rgba(0,0,0,0.05); border: 1px solid #e2e8f0; position: relative; transition: 0.3s; }
        .card:hover { transform: translateY(-5px); box-shadow: 0 10px 25px rgba(0,0,0,0.1); }
        
        /* 3D Delete Button for Inventory */
        .delete-item-btn { position: absolute; top: 10px; right: 10px; background: #fee2e2; color: #ef4444; border-radius: 50%; width: 30px; height: 30px; display: flex; align-items: center; justify-content: center; font-size: 0.8rem; box-shadow: 0 4px 0 #fca5a5; transition: 0.1s; }
        .delete-item-btn:active { transform: translateY(3px); box-shadow: 0 1px 0 #fca5a5; }

        .card img { width: 100%; height: 180px; object-fit: cover; border-radius: 10px; margin-bottom: 12px; }
        .card h3 { font-size: 1rem; text-transform: uppercase; margin-bottom: 5px; color: #1e293b; }
        .card .price-tag { color: var(--accent); font-weight: 800; display: block; margin-bottom: 12px; font-size: 1.2rem; }
        .btn-order { background: #1e293b; color: white; width: 100%; padding: 10px; border-radius: 10px; }

        /* Order Section */
        .order-section { background: rgba(255,255,255,0.07); border-radius: 20px; padding: 25px; border: 1px solid rgba(255,255,255,0.1); height: fit-content; }
        .order-section h2 { font-family: 'Montserrat'; color: #f1c40f; margin-bottom: 20px; text-transform: uppercase; letter-spacing: 2px; }
        
        .order-item { background: white; color: #333; padding: 15px; border-radius: 12px; margin-bottom: 12px; display: flex; align-items: center; gap: 12px; position: relative; }
        .order-item img { width: 55px; height: 55px; border-radius: 8px; object-fit: cover; }
        .order-details { flex: 1; }
        .order-details h4 { font-size: 0.85rem; font-weight: 700; }
        
        .qty-controls { display: flex; align-items: center; gap: 8px; margin-top: 5px; }
        .qty-btn { background: #f1f5f9; width: 22px; height: 22px; border-radius: 4px; display: flex; align-items: center; justify-content: center; font-size: 0.9rem; font-weight: bold; }
        .qty-val { font-weight: bold; font-size: 0.9rem; }

        .remove-order-btn { background: #ef4444; color: white; padding: 5px 8px; border-radius: 6px; font-size: 0.65rem; margin-left: 10px; }

        /* Totals Footer */
        .order-footer { margin-top: 20px; padding-top: 20px; border-top: 2px solid rgba(255,255,255,0.1); }
        .total-row { display: flex; justify-content: space-between; font-size: 1.5rem; font-weight: 800; color: #f1c40f; }
        .btn-save-all { background: #3b82f6; color: white; width: 100%; padding: 15px; margin-top: 15px; text-transform: uppercase; font-size: 0.9rem; letter-spacing: 1px; box-shadow: 0 5px 0 #2563eb; }
        .btn-save-all:active { transform: translateY(4px); box-shadow: 0 1px 0 #2563eb; }

        /* Receipt (Hidden Area) */
        #receipt-area { position: absolute; left: -9999px; background: white; width: 600px; padding: 50px; color: #333; }
        .r-header { text-align: center; border-bottom: 3px solid #333; padding-bottom: 20px; margin-bottom: 20px; }
        .r-header h1 { font-family: 'Playfair Display'; font-size: 3rem; background: var(--gold); -webkit-background-clip: text; -webkit-text-fill-color: transparent; }
        .r-table { width: 100%; border-collapse: collapse; }
        .r-table th { text-align: left; padding: 10px; border-bottom: 2px solid #333; }
        .r-table td { padding: 10px; border-bottom: 1px solid #eee; }
    </style>
</head>
<body>

    <div class="dashboard">
        <input type="text" id="search" style="width:100%; padding:15px; border-radius:12px; margin-bottom:15px;" placeholder="🔍 Search Products in Inventory..." onkeyup="filterProducts()">
        <div class="admin-row">
            <input type="file" id="p-img" accept="image/*">
            <input type="text" id="p-name" placeholder="New Product Name">
            <input type="number" id="p-price" placeholder="Price (Numbers)">
            <button class="btn-add" onclick="addProduct()">➕ Add Item</button>
            <button class="btn-dl" onclick="downloadCatalog()">📁 Export Catalog</button>
        </div>
    </div>

    <div class="main-container">
        <div class="catalog-section">
            <div class="grid" id="catalog-grid"></div>
        </div>

        <div class="order-section">
            <h2>Orders List</h2>
            <div id="order-list">
                <p style="color: #94a3b8; text-align: center; padding: 20px;">Cart is empty. Order something!</p>
            </div>
            
            <div class="order-footer">
                <div class="total-row">
                    <span>GRAND TOTAL:</span>
                    <span id="grand-total">PHP 0.00</span>
                </div>
                <button class="btn-save-all" onclick="saveOrderReceipt()">💾 Save Receipt Image</button>
            </div>
        </div>
    </div>

    <div id="receipt-area">
        <div class="r-header">
            <h1>SmartGrade Solutions</h1>
            <p>High Performance • Minor Flaws • Major Savings</p>
        </div>
        <table class="r-table">
            <thead>
                <tr>
                    <th>Description</th>
                    <th>Qty</th>
                    <th>Price</th>
                    <th>Subtotal</th>
                </tr>
            </thead>
            <tbody id="r-items"></tbody>
        </table>
        <div id="r-total" style="text-align:right; font-size: 2rem; font-weight: 800; color: #ef4444; margin-top: 30px;"></div>
        <p style="text-align:center; margin-top:50px; font-weight:bold; border-top:2px dashed #eee; padding-top:20px;">CONFIRMED ORDER - Barato Ra!</p>
    </div>

    <script>
        let inventory = [
            { id: 1, name: "Clear Gloss Varnish", price: 450, img: "https://images.unsplash.com/photo-1589939705384-5185137a7f0f?w=400" },
            { id: 2, name: "Epoxy Resin Blue", price: 1200, img: "https://images.unsplash.com/photo-1562259949-e8e7689d7828?w=400" }
        ];

        let cart = [];

        function renderInventory(data = inventory) {
            const grid = document.getElementById('catalog-grid');
            grid.innerHTML = data.map(item => `
                <div class="card">
                    <button class="delete-item-btn" onclick="deleteFromInventory(${item.id})">✖</button>
                    <img src="${item.img}">
                    <h3>${item.name}</h3>
                    <span class="price-tag">PHP ${item.price.toLocaleString()}</span>
                    <button class="btn-order" onclick="addToCart(${item.id})">Order Now</button>
                </div>
            `).join('');
        }

        function addProduct() {
            const name = document.getElementById('p-name').value;
            const price = parseFloat(document.getElementById('p-price').value);
            const file = document.getElementById('p-img').files[0];
            if(!name || !price || !file) return alert("Fill all details first!");

            const reader = new FileReader();
            reader.onload = (e) => {
                inventory.push({ id: Date.now(), name, price, img: e.target.result });
                renderInventory();
                document.getElementById('p-name').value = '';
                document.getElementById('p-price').value = '';
                document.getElementById('p-img').value = '';
            };
            reader.readAsDataURL(file);
        }

        function deleteFromInventory(id) {
            if(confirm("Remove this item from your Shop permanently?")) {
                inventory = inventory.filter(i => i.id !== id);
                renderInventory();
            }
        }

        function filterProducts() {
            const q = document.getElementById('search').value.toLowerCase();
            const filtered = inventory.filter(i => i.name.toLowerCase().includes(q));
            renderInventory(filtered);
        }

        function addToCart(id) {
            const item = inventory.find(i => i.id === id);
            const existing = cart.find(c => c.id === id);
            if(existing) {
                existing.qty++;
            } else {
                cart.push({ ...item, qty: 1 });
            }
            updateCartUI();
        }

        function changeQty(index, delta) {
            cart[index].qty += delta;
            if(cart[index].qty <= 0) cart.splice(index, 1);
            updateCartUI();
        }

        function removeOrder(index) {
            cart.splice(index, 1);
            updateCartUI();
        }

        function updateCartUI() {
            const list = document.getElementById('order-list');
            const totalEl = document.getElementById('grand-total');
            let grandTotal = 0;

            if(cart.length === 0) {
                list.innerHTML = `<p style="color: #94a3b8; text-align: center; padding: 20px;">Cart is empty.</p>`;
                totalEl.innerText = "PHP 0.00";
                return;
            }

            list.innerHTML = cart.map((item, i) => {
                const sub = item.price * item.qty;
                grandTotal += sub;
                return `
                    <div class="order-item">
                        <img src="${item.img}">
                        <div class="order-details">
                            <h4>${item.name}</h4>
                            <div class="qty-controls">
                                <button class="qty-btn" onclick="changeQty(${i}, -1)">-</button>
                                <span class="qty-val">${item.qty} pcs</span>
                                <button class="qty-btn" onclick="changeQty(${i}, 1)">+</button>
                                <button class="remove-order-btn" onclick="removeOrder(${i})">Delete</button>
                            </div>
                        </div>
                        <div style="font-weight:800; color:#1e293b;">PHP ${sub.toLocaleString()}</div>
                    </div>
                `;
            }).join('');

            totalEl.innerText = `PHP ${grandTotal.toLocaleString()}`;
        }

        async function saveOrderReceipt() {
            if(cart.length === 0) return alert("Cart is empty!");
            const tbody = document.getElementById('r-items');
            let total = 0;
            tbody.innerHTML = cart.map(item => {
                const sub = item.price * item.qty;
                total += sub;
                return `<tr><td>${item.name}</td><td>${item.qty}</td><td>${item.price}</td><td>PHP ${sub.toLocaleString()}</td></tr>`;
            }).join('');
            document.getElementById('r-total').innerText = `TOTAL: PHP ${total.toLocaleString()}`;

            const canvas = await html2canvas(document.getElementById('receipt-area'), { scale: 2 });
            const link = document.createElement('a');
            link.download = `SmartGrade_Receipt_${Date.now()}.png`;
            link.href = canvas.toDataURL("image/png");
            link.click();
        }

        async function downloadCatalog() {
            const area = document.querySelector('.catalog-section');
            const canvas = await html2canvas(area, { scale: 2 });
            const link = document.createElement('a');
            link.download = 'SmartGrade_Inventory.png';
            link.href = canvas.toDataURL("image/png");
            link.click();
        }

        renderInventory();
    </script>
</body>
</html>
