# K-CAFE
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>K Cafe - Billing System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.23/jspdf.plugin.autotable.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-image: url('https://images.unsplash.com/photo-1559925393-8be0ec4767c8?q=80&w=2070&auto=format&fit=crop');
            background-size: cover;
            background-position: center;
            background-attachment: fixed;
        }
        .invoice-container {
            max-width: 900px;
            margin: 2rem auto;
            padding: 2rem;
            box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);
            border-radius: 12px;
            background-color: rgba(255, 255, 255, 0.9);
            border: 1px solid rgba(255, 255, 255, 0.18);
            backdrop-filter: blur(10px);
            -webkit-backdrop-filter: blur(10px);
        }
        .btn {
            transition: all 0.3s ease;
        }
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }
        .remove-btn {
            color: #ef4444;
        }
        .remove-btn:hover {
            color: #b91c1c;
            background-color: #fee2e2;
        }
        @media print {
            body {
                background-image: none;
            }
            body * {
                visibility: hidden;
            }
            .invoice-container, .invoice-container * {
                visibility: visible;
            }
            .invoice-container {
                position: absolute;
                left: 0;
                top: 0;
                width: 100%;
                box-shadow: none;
                margin: 0;
                padding: 0;
                background-color: #ffffff;
                backdrop-filter: none;
            }
            .no-print {
                display: none;
            }
        }
    </style>
</head>
<body class="bg-gray-100">

    <div class="invoice-container">
        <!-- Header with Logo -->
        <header class="text-center mb-8 border-b pb-4 border-gray-300">
            <img src="https://placehold.co/120x120/f7f2e9/6b4f4b?text=K-Cafe" alt="K Cafe Logo" class="mx-auto mb-4 rounded-full border-2 border-white shadow-md">
            <h1 class="text-3xl font-bold text-gray-800">K Cafe</h1>
            <p class="text-gray-600">Event Billing Invoice</p>
        </header>

        <!-- Invoice Details -->
        <div class="flex justify-between mb-8">
            <div>
                <label for="invoiceNumber" class="block text-sm font-medium text-gray-700">Invoice Number:</label>
                <input type="text" id="invoiceNumber" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm" value="INV-001">
            </div>
            <div>
                <label for="invoiceDate" class="block text-sm font-medium text-gray-700">Date:</label>
                <input type="date" id="invoiceDate" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
            </div>
        </div>

        <!-- Add Item Form -->
        <div class="bg-white/50 p-6 rounded-lg mb-8 no-print shadow-inner">
            <h2 class="text-xl font-semibold mb-4 text-gray-700">Item Shamil Karein</h2>
            <div class="grid grid-cols-1 md:grid-cols-5 gap-4 items-end">
                <div class="md:col-span-2">
                    <label for="productName" class="block text-sm font-medium text-gray-700">Product</label>
                    <select id="productName" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                        <option>Water Bottle</option>
                        <option>Cold Drink</option>
                        <option>Chai</option>
                        <option>Other</option>
                    </select>
                </div>
                <div>
                    <label for="productSize" class="block text-sm font-medium text-gray-700">Size</label>
                    <input type="text" id="productSize" placeholder="e.g., 500ml, Regular" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                </div>
                <div>
                    <label for="productQty" class="block text-sm font-medium text-gray-700">Quantity</label>
                    <input type="number" id="productQty" value="1" min="1" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                </div>
                <div>
                    <label for="productPrice" class="block text-sm font-medium text-gray-700">Price (per item)</label>
                    <input type="number" id="productPrice" placeholder="0.00" min="0" step="0.01" class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-indigo-500 focus:ring-indigo-500 sm:text-sm">
                </div>
            </div>
             <div class="flex justify-end mt-4">
                 <button id="addItemBtn" class="btn bg-indigo-600 text-white font-bold py-2 px-6 rounded-lg hover:bg-indigo-700">Add Item</button>
            </div>
        </div>

        <!-- Items Table -->
        <div class="mt-8">
            <h3 class="text-lg font-semibold text-gray-800 mb-2">Bill Items</h3>
            <div class="overflow-x-auto">
                <table class="min-w-full bg-white/70 rounded-lg overflow-hidden">
                    <thead class="bg-gray-200/80">
                        <tr>
                            <th class="text-left py-3 px-4 font-semibold text-sm text-gray-600">Product</th>
                            <th class="text-left py-3 px-4 font-semibold text-sm text-gray-600">Size</th>
                            <th class="text-center py-3 px-4 font-semibold text-sm text-gray-600">Quantity</th>
                            <th class="text-right py-3 px-4 font-semibold text-sm text-gray-600">Unit Price</th>
                            <th class="text-right py-3 px-4 font-semibold text-sm text-gray-600">Total</th>
                            <th class="text-center py-3 px-4 font-semibold text-sm text-gray-600 no-print">Action</th>
                        </tr>
                    </thead>
                    <tbody id="billingItems">
                        <!-- Items will be added here dynamically -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Total Section -->
        <div class="mt-8 flex justify-end">
            <div class="w-full max-w-xs">
                <div class="flex justify-between py-2 border-b border-gray-300">
                    <span class="font-medium text-gray-600">Subtotal</span>
                    <span id="subtotal" class="font-semibold text-gray-800">Rs 0.00</span>
                </div>
                <div class="flex justify-between py-2 border-b border-gray-300">
                    <span class="font-medium text-gray-600">Tax (0%)</span>
                    <span id="tax" class="font-semibold text-gray-800">Rs 0.00</span>
                </div>
                <div class="flex justify-between py-3 bg-gray-100/80 px-2 rounded-md mt-2">
                    <span class="font-bold text-lg text-gray-900">Total</span>
                    <span id="total" class="font-bold text-lg text-gray-900">Rs 0.00</span>
                </div>
            </div>
        </div>

        <!-- Action Button -->
        <div class="mt-12 text-center no-print">
            <button id="generatePdfBtn" class="btn bg-green-600 text-white font-bold py-3 px-8 rounded-lg hover:bg-green-700">
                Generate & Download PDF
            </button>
        </div>
    </div>
    
    <!-- Error Modal -->
    <div id="errorModal" class="fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full hidden z-50">
        <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
            <div class="mt-3 text-center">
                <h3 class="text-lg leading-6 font-medium text-gray-900">Alert</h3>
                <div class="mt-2 px-7 py-3">
                    <p id="modalMessage" class="text-sm text-gray-500"></p>
                </div>
                <div class="items-center px-4 py-3">
                    <button id="closeModalBtn" class="px-4 py-2 bg-indigo-500 text-white text-base font-medium rounded-md w-full shadow-sm hover:bg-indigo-600 focus:outline-none focus:ring-2 focus:ring-indigo-300">
                        Close
                    </button>
                </div>
            </div>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', function () {
            // Elements
            const addItemBtn = document.getElementById('addItemBtn');
            const billingItems = document.getElementById('billingItems');
            const subtotalEl = document.getElementById('subtotal');
            const taxEl = document.getElementById('tax');
            const totalEl = document.getElementById('total');
            const generatePdfBtn = document.getElementById('generatePdfBtn');
            const invoiceDateEl = document.getElementById('invoiceDate');
            const errorModal = document.getElementById('errorModal');
            const modalMessage = document.getElementById('modalMessage');
            const closeModalBtn = document.getElementById('closeModalBtn');

            // Set current date
            invoiceDateEl.valueAsDate = new Date();

            let items = [];
            let itemId = 0;

            // --- Modal Functions ---
            function showModal(message) {
                modalMessage.textContent = message;
                errorModal.classList.remove('hidden');
            }

            closeModalBtn.addEventListener('click', () => {
                errorModal.classList.add('hidden');
            });

            // --- Billing Logic ---
            addItemBtn.addEventListener('click', () => {
                const name = document.getElementById('productName').value;
                const size = document.getElementById('productSize').value || '-';
                const qty = parseFloat(document.getElementById('productQty').value);
                const price = parseFloat(document.getElementById('productPrice').value);

                if (name && qty > 0 && price >= 0) {
                    const newItem = {
                        id: itemId++,
                        name,
                        size,
                        qty,
                        price,
                        total: qty * price
                    };
                    items.push(newItem);
                    renderTable();
                    updateTotals();
                    clearForm();
                } else {
                    showModal('Please fill all fields correctly.');
                }
            });

            function renderTable() {
                billingItems.innerHTML = '';
                items.forEach(item => {
                    const row = document.createElement('tr');
                    row.className = 'border-b border-gray-300/50';
                    row.innerHTML = `
                        <td class="py-3 px-4">${item.name}</td>
                        <td class="py-3 px-4">${item.size}</td>
                        <td class="text-center py-3 px-4">${item.qty}</td>
                        <td class="text-right py-3 px-4">Rs ${item.price.toFixed(2)}</td>
                        <td class="text-right py-3 px-4 font-semibold">Rs ${item.total.toFixed(2)}</td>
                        <td class="text-center py-3 px-4 no-print">
                            <button class="remove-btn p-1 rounded-full" data-id="${item.id}">
                                <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
                            </button>
                        </td>
                    `;
                    billingItems.appendChild(row);
                });

                document.querySelectorAll('.remove-btn').forEach(button => {
                    button.addEventListener('click', (e) => {
                        const idToRemove = parseInt(e.currentTarget.getAttribute('data-id'));
                        items = items.filter(item => item.id !== idToRemove);
                        renderTable();
                        updateTotals();
                    });
                });
            }
            
            function updateTotals() {
                const subtotal = items.reduce((acc, item) => acc + item.total, 0);
                const tax = 0;
                const total = subtotal + tax;

                subtotalEl.textContent = `Rs ${subtotal.toFixed(2)}`;
                taxEl.textContent = `Rs ${tax.toFixed(2)}`;
                totalEl.textContent = `Rs ${total.toFixed(2)}`;
            }
            
            function clearForm() {
                document.getElementById('productName').selectedIndex = 0;
                document.getElementById('productSize').value = '';
                document.getElementById('productQty').value = '1';
                document.getElementById('productPrice').value = '';
            }
            
            // --- PDF Generation with Logo ---
            generatePdfBtn.addEventListener('click', () => {
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF();
                const invoiceNumber = document.getElementById('invoiceNumber').value;
                const invoiceDate = document.getElementById('invoiceDate').value;

                const logoImg = new Image();
                logoImg.crossOrigin = 'Anonymous';

                logoImg.onload = function() {
                    const canvas = document.createElement('canvas');
                    canvas.width = this.naturalWidth;
                    canvas.height = this.naturalHeight;
                    const ctx = canvas.getContext('2d');
                    ctx.drawImage(this, 0, 0);
                    const dataUrl = canvas.toDataURL('image/png');
                    
                    doc.addImage(dataUrl, 'PNG', 95, 10, 20, 20); 

                    doc.setFontSize(22);
                    doc.setFont('helvetica', 'bold');
                    doc.text("K Cafe", 105, 40, null, null, "center");
                    doc.setFontSize(12);
                    doc.setFont('helvetica', 'normal');
                    doc.text("Event Billing Invoice", 105, 48, null, null, "center");

                    doc.setFontSize(10);
                    doc.text(`Invoice #: ${invoiceNumber}`, 14, 60);
                    doc.text(`Date: ${invoiceDate}`, 14, 65);
                    
                    const tableColumn = ["Product", "Size", "Quantity", "Unit Price", "Total"];
                    const tableRows = [];
                    items.forEach(item => {
                        tableRows.push([
                            item.name,
                            item.size,
                            item.qty,
                            `Rs ${item.price.toFixed(2)}`,
                            `Rs ${item.total.toFixed(2)}`
                        ]);
                    });

                    doc.autoTable({
                        head: [tableColumn],
                        body: tableRows,
                        startY: 75,
                        theme: 'grid',
                        headStyles: { fillColor: [100, 68, 60] }, // Brown color for header
                        styles: { halign: 'center' },
                        columnStyles: {
                            0: { halign: 'left' }, 1: { halign: 'left' },
                            3: { halign: 'right' }, 4: { halign: 'right' }
                        }
                    });
                    
                    let finalY = doc.lastAutoTable.finalY || 80;

                    doc.setFontSize(12);
                    doc.text(`Subtotal: ${subtotalEl.textContent}`, 14, finalY + 15);
                    doc.setFont('helvetica', 'bold');
                    doc.text(`Total: ${totalEl.textContent}`, 14, finalY + 22);

                    doc.setFontSize(10);
                    doc.setTextColor(150);
                    doc.text("Thank you for your business!", 105, doc.internal.pageSize.height - 10, null, null, "center");

                    doc.save(`KCafe-Invoice-${invoiceNumber}.pdf`);
                };
                
                logoImg.onerror = function() {
                    showModal("Could not load logo image for PDF. The PDF will be generated without it.");
                };

                logoImg.src = 'https://placehold.co/200x200/f7f2e9/6b4f4b?text=K-Cafe';
            });
        });
    </script>
</body>
</html>
