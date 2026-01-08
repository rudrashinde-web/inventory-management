# inventory-management
first project
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Inventory Management</title>
  <style>
    body { font-family: 'Segoe UI', Arial, sans-serif; margin: 40px; background-color: #f4f7f6; }
    .container { max-width: 1000px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 2px 10px rgba(0,0,0,0.1); }
    table { width: 100%; border-collapse: collapse; margin-top: 20px; }
    th, td { border: 1px solid #ddd; padding: 12px; text-align: left; }
    th { background-color: #007bff; color: white; }
    form { margin-bottom: 20px; display: flex; gap: 10px; flex-wrap: wrap; align-items: flex-end; }
    .input-group { display: flex; flex-direction: column; }
    label { font-size: 12px; font-weight: bold; margin-bottom: 5px; color: #666; }
    select { padding: 8px; border-radius: 4px; border: 1px solid #ccc; min-width: 150px; }
    button { padding: 10px 15px; background-color: #28a745; color: white; border: none; border-radius: 4px; cursor: pointer; font-weight: bold; }
    button:hover { background-color: #218838; }
    .delete-btn { background-color: #dc3545; border-radius: 4px; padding: 6px 10px; color: white; border: none; cursor: pointer; }
    .date-text { font-size: 0.85rem; color: #666; }
    .clear-storage { background-color: #6c757d; margin-top: 20px; font-size: 10px; opacity: 0.6; color: white; border: none; padding: 5px; cursor: pointer; }
  </style>
</head>
<body>

<div class="container">
  <h1>Inventory Management</h1>

  <h2>Add New Entry</h2>
  <form id="addForm">
    <div class="input-group">
      <label>Item Name</label>
      <select id="name" required>
        <option value="">Select Code</option>
        <option value="IB">IB</option>
        <option value="RS">RS</option>
        <option value="OC">OC</option>
        <option value="MD">MD</option>
        <option value="KS">KS</option>
        <option value="GF">GF</option>
        <option value="DE">DE</option>
      </select>
    </div>

    <div class="input-group">
      <label>Quantity</label>
      <select id="quantity" required>
        <option value="">Select Qty</option>
        <option value="1">1</option>
        <option value="2">2</option>
        <option value="6">6</option>
        <option value="10">10</option>
        <option value="12">12</option>
        <option value="24">24</option>
        <option value="48">48</option>
        <option value="96">96</option>
      </select>
    </div>

    <div class="input-group">
      <label>Price</label>
      <select id="price" required>
        <option value="">Select Price</option>
        <option value="250">250</option>
        <option value="260">260</option>
        <option value="210">210</option>
        <option value="240">240</option>
        <option value="175">175</option>
        <option value="230">230</option>
        <option value="310">310</option>
      </select>
    </div>
    
    <button type="submit" id="submitBtn">Add Item</button>
  </form>

  <hr>

  <h2>Current Inventory</h2>
  <table id="itemsTable">
    <thead>
      <tr>
        <th>item Added</th>
        <th>Name</th>
        <th>Quantity</th>
        <th>Price</th>
        <th>Total Value</th>
        <th>Actions</th>
      </tr>
    </thead>
    <tbody></tbody>
  </table>

  <button class="clear-storage" onclick="clearAllData()">Reset System Memory</button>
</div>

<script>
    let inventory = [];
    
    // Load existing data
    try {
        const savedData = localStorage.getItem('myInventoryData');
        if (savedData) {
            inventory = JSON.parse(savedData);
        }
    } catch (e) {
        inventory = [];
    }

    const tableBody = document.querySelector('#itemsTable tbody');
    const addForm = document.getElementById('addForm');

    function displayItems() {
      tableBody.innerHTML = '';
      
      inventory.forEach((item, index) => {
        const row = document.createElement('tr');
        const q = parseInt(item.quantity) || 0;
        const p = parseFloat(item.price) || 0;
        const totalValue = q * p;

        row.innerHTML = `
          <td class="date-text">${item.date || 'N/A'}</td>
          <td style="text-transform: uppercase; font-weight: bold;">${item.name}</td>
          <td>${q}</td>
          <td>$${p}</td>
          <td>$${totalValue.toLocaleString()}</td>
          <td>
            <button class="delete-btn" onclick="deleteItem(${index})">Delete</button>
          </td>
        `;
        tableBody.appendChild(row);
      });
      
      localStorage.setItem('myInventoryData', JSON.stringify(inventory));
    }

    addForm.onsubmit = function(e) {
      e.preventDefault();
      
      const nameVal = document.getElementById('name').value;
      const qtyVal = document.getElementById('quantity').value;
      const priceVal = document.getElementById('price').value;
      
      // Get current date and time
      const now = new Date();
      const dateString = now.toLocaleDateString() + ' ' + now.toLocaleTimeString([], {hour: '2-digit', minute:'2-digit'});

      if(nameVal && qtyVal && priceVal) {
          const newItem = {
            date: dateString,
            name: nameVal,
            quantity: qtyVal,
            price: priceVal
          };

          inventory.push(newItem);
          displayItems();
          addForm.reset();
      }
    };

    window.deleteItem = function(index) {
      if(confirm("Remove this item?")) {
        inventory.splice(index, 1);
        displayItems();
      }
    };

    function clearAllData() {
        if(confirm("This will delete EVERYTHING. Are you sure?")) {
            localStorage.clear();
            inventory = [];
            displayItems();
        }
    }

    displayItems();
</script>
</body>
</html>
