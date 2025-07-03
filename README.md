<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>K Cafe - Advanced Billing System</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.23/jspdf.plugin.autotable.min.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Poppins', sans-serif;
            background-color: #f4f1eb;
        }
        .invoice-container {
            max-width: 1000px;
            margin: 1rem auto;
            padding: 1.5rem;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.1);
            border-radius: 15px;
            background-color: #ffffff;
            border-top: 5px solid #6b4f4b;
        }
        @media (min-width: 768px) {
            .invoice-container {
                margin: 2rem auto;
                padding: 2.5rem;
            }
        }
        .btn {
            transition: all 0.3s ease;
            border-radius: 8px;
        }
        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        .remove-btn:hover {
            color: #b91c1c;
            background-color: #fee2e2;
        }
        .qty-btn {
            background-color: #f3f4f6;
            color: #4b5563;
            border-radius: 50%;
            width: 28px;
            height: 28px;
            font-weight: 600;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            transition: all 0.2s;
            border: 1px solid #e5e7eb;
        }
        .qty-btn:hover {
            background-color: #e5e7eb;
            border-color: #d1d5db;
        }
        input[type="number"]::-webkit-inner-spin-button, 
        input[type="number"]::-webkit-outer-spin-button {
            -webkit-appearance: none;
            margin: 0;
        }
        input[type="number"] {
            -moz-appearance: textfield;
        }
        .form-input {
            border-radius: 8px;
            border: 1px solid #d1d5db;
            transition: border-color 0.2s, box-shadow 0.2s;
        }
        .form-input:focus {
            border-color: #6b4f4b;
            box-shadow: 0 0 0 2px rgba(107, 79, 75, 0.2);
            outline: none;
        }
        @media print {
            body { background-image: none; background-color: #fff; }
            body * { visibility: hidden; }
            .invoice-container, .invoice-container * { visibility: visible; }
            .invoice-container { position: absolute; left: 0; top: 0; width: 100%; box-shadow: none; margin: 0; padding: 0; border: none; }
            .no-print { display: none; }
        }
    </style>
</head>
<body>

    <div class="invoice-container">
        <!-- Header -->
        <header class="flex flex-col sm:flex-row justify-between items-start mb-8 pb-4 border-b">
            <div class="flex items-center">
                <img src="https://placehold.co/80x80/f4f1eb/6b4f4b?text=K-Cafe" alt="K Cafe Logo" class="rounded-full mr-4">
                <div>
                    <h1 class="text-3xl md:text-4xl font-bold text-gray-800">K Cafe</h1>
                    <p class="text-gray-500">Event Billing</p>
                </div>
            </div>
            <div class="text-left sm:text-right mt-4 sm:mt-0 w-full sm:w-auto">
                <h2 class="text-2xl font-semibold uppercase text-gray-700">Invoice</h2>
                <div class="mt-2">
                    <label for="invoiceNumber" class="text-sm font-medium text-gray-600">Invoice #</label>
                    <input type="text" id="invoiceNumber" class="form-input mt-1 p-1 w-full sm:w-40 text-left sm:text-right font-semibold text-gray-800" readonly>
                </div>
                <div class="mt-2">
                    <label for="invoiceDate" class="text-sm font-medium text-gray-600">Date</label>
                    <input type="date" id="invoiceDate" class="form-input mt-1 p-1 w-full sm:w-40 text-left sm:text-right">
                </div>
            </div>
        </header>

        <!-- Client Info -->
        <div class="bg-gray-50 p-4 rounded-lg mb-8">
            <h3 class="font-semibold text-gray-700 mb-2">Bill To:</h3>
            <input type="text" id="clientName" placeholder="Client Name" class="form-input w-full p-2 mb-2">
            <input type="text" id="clientContact" placeholder="Client Contact" class="form-input w-full p-2">
        </div>

        <!-- Add Item Form -->
        <div class="bg-gray-50 p-6 rounded-lg mb-8 no-print">
            <h3 class="text-xl font-semibold mb-4 text-gray-700">Item Shamil Karein</h3>
            <div class="grid grid-cols-1 md:grid-cols-12 gap-4 items-end">
                <div class="md:col-span-4">
                    <label for="productName" class="block text-sm font-medium text-gray-700">Product</label>
                    <input type="text" id="productName" list="productList" placeholder="Product ka naam type karein" class="form-input mt-1 block w-full p-2">
                    <datalist id="productList">
                        <option>Water Bottle</option>
                        <option>Cold Drink</option>
                        <option>Chai</option>
                    </datalist>
                </div>
                <div class="md:col-span-3">
                    <label for="productSize" class="block text-sm font-medium text-gray-700">Size / Description</label>
                    <input type="text" id="productSize" placeholder="e.g., 500ml, Regular" class="form-input mt-1 block w-full p-2">
                </div>
                <div class="md:col-span-2">
                    <label for="productQty" class="block text-sm font-medium text-gray-700">Quantity</label>
                    <input type="number" id="productQty" value="1" min="1" class="form-input mt-1 block w-full p-2 text-center">
                </div>
                <div class="md:col-span-2">
                    <label for="productPrice" class="block text-sm font-medium text-gray-700">Price</label>
                    <input type="number" id="productPrice" placeholder="0.00" min="0" step="0.01" class="form-input mt-1 block w-full p-2 text-right">
                </div>
                <div class="md:col-span-1">
                    <button id="addItemBtn" class="btn bg-[#6b4f4b] text-white font-bold py-2 w-full flex justify-center items-center h-[42px] hover:bg-[#5a423d]">
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 4v16m8-8H4" /></svg>
                    </button>
                </div>
            </div>
        </div>

        <!-- Items Table -->
        <div class="mt-8">
            <div class="overflow-x-auto">
                <table class="min-w-full">
                    <thead class="border-b-2 border-gray-300">
                        <tr>
                            <th class="text-left py-3 px-2 sm:px-4 font-semibold text-sm text-gray-600 uppercase">Product</th>
                            <th class="text-center py-3 px-2 sm:px-4 font-semibold text-sm text-gray-600 uppercase">Quantity</th>
                            <th class="text-right py-3 px-2 sm:px-4 font-semibold text-sm text-gray-600 uppercase">Unit Price</th>
                            <th class="text-right py-3 px-2 sm:px-4 font-semibold text-sm text-gray-600 uppercase">Total</th>
                            <th class="text-center py-3 px-2 sm:px-4 font-semibold text-sm text-gray-600 uppercase no-print"></th>
                        </tr>
                    </thead>
                    <tbody id="billingItems" class="divide-y divide-gray-200">
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Total & Notes Section -->
        <div class="mt-8 grid grid-cols-1 md:grid-cols-2 gap-8">
            <div>
                <h4 class="font-semibold text-gray-700 mb-2">Terms & Conditions:</h4>
                <textarea id="notes" class="form-input w-full p-2 text-sm text-gray-600" rows="5"></textarea>
            </div>
            <div class="space-y-3">
                <div class="flex justify-between items-center">
                    <span class="font-medium text-gray-600">Subtotal</span>
                    <span id="subtotal" class="font-semibold text-gray-800 text-lg">Rs 0.00</span>
                </div>
                <div class="flex justify-between items-center">
                    <span class="font-medium text-gray-600">Discount (Rs)</span>
                    <input type="number" id="discountInput" class="form-input w-28 p-1 text-right" value="0" min="0">
                </div>
                <div class="flex justify-between items-center">
                    <span class="font-medium text-gray-600">Tax (%)</span>
                    <input type="number" id="taxInput" class="form-input w-28 p-1 text-right" value="0" min="0">
                </div>
                <div class="flex justify-between items-center border-t-2 pt-3 mt-3">
                    <span class="font-bold text-xl text-gray-900">Total</span>
                    <span id="total" class="font-bold text-xl text-gray-900">Rs 0.00</span>
                </div>
            </div>
        </div>

        <!-- Action Button -->
        <div class="mt-12 text-center no-print">
            <button id="generatePdfBtn" class="btn bg-green-600 text-white font-bold py-3 px-8 hover:bg-green-700">
                Generate & Download PDF
            </button>
        </div>
    </div>
    
    <div id="errorModal" class="fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full hidden z-50">
        <div class="relative top-20 mx-auto p-5 border w-96 shadow-lg rounded-md bg-white">
            <div class="mt-3 text-center">
                <h3 class="text-lg leading-6 font-medium text-gray-900">Alert</h3>
                <div class="mt-2 px-7 py-3"><p id="modalMessage" class="text-sm text-gray-500"></p></div>
                <div class="items-center px-4 py-3"><button id="closeModalBtn" class="px-4 py-2 bg-indigo-500 text-white text-base font-medium rounded-md w-full shadow-sm hover:bg-indigo-600 focus:outline-none">Close</button></div>
            </div>
        </div>
    </div>

    <script>
    document.addEventListener('DOMContentLoaded', function () {
        const addItemBtn = document.getElementById('addItemBtn');
        const billingItems = document.getElementById('billingItems');
        const subtotalEl = document.getElementById('subtotal');
        const totalEl = document.getElementById('total');
        const discountInput = document.getElementById('discountInput');
        const taxInput = document.getElementById('taxInput');
        const notesEl = document.getElementById('notes');
        const generatePdfBtn = document.getElementById('generatePdfBtn');
        const invoiceDateEl = document.getElementById('invoiceDate');
        const invoiceNumberEl = document.getElementById('invoiceNumber');
        const errorModal = document.getElementById('errorModal');
        const modalMessage = document.getElementById('modalMessage');
        const closeModalBtn = document.getElementById('closeModalBtn');

        let items = [];
        let itemId = 0;

        function setInitialValues() {
            const today = new Date();
            invoiceDateEl.value = today.toISOString().split('T')[0];
            const dateStr = today.toISOString().slice(0, 10).replace(/-/g, "");
            invoiceNumberEl.value = `KCF-${dateStr}-${String(Math.floor(Math.random() * 1000) + 1).padStart(3, '0')}`;
            
            const termsAndConditions = [
                "- Payment will be made in advance.",
                "- No returns will be accepted.",
                "- Water can be of any brand: Pakola or Abae Dubai.",
                "- If rates are changed by the company, the same will apply here."
            ].join('\n');
            notesEl.value = termsAndConditions;
        }

        function showModal(message) {
            modalMessage.textContent = message;
            errorModal.classList.remove('hidden');
        }
        closeModalBtn.addEventListener('click', () => errorModal.classList.add('hidden'));

        function addItem() {
            const name = document.getElementById('productName').value;
            const size = document.getElementById('productSize').value || '-';
            const qty = parseFloat(document.getElementById('productQty').value);
            const price = parseFloat(document.getElementById('productPrice').value);

            if (name && qty > 0 && price >= 0) {
                const existingItem = items.find(item => item.name === name && item.size === size);
                if (existingItem) {
                    showModal('This item is already in the list. Please adjust its quantity directly in the table.');
                    return;
                }
                const newItem = { id: itemId++, name, size, qty, price, total: qty * price };
                items.push(newItem);
                renderTable();
                clearForm();
            } else {
                showModal('Please fill all product fields correctly.');
            }
        }

        function updateItemQuantity(id, newQty) {
            const item = items.find(i => i.id === id);
            if (item) {
                if (newQty <= 0) {
                    items = items.filter(i => i.id !== id);
                } else {
                    item.qty = newQty;
                    item.total = item.qty * item.price;
                }
                renderTable();
            }
        }

        function renderTable() {
            billingItems.innerHTML = '';
            items.forEach(item => {
                const row = document.createElement('tr');
                row.className = 'item-row';
                row.innerHTML = `
                    <td class="py-3 px-2 sm:px-4">
                        <p class="font-semibold text-gray-800">${item.name}</p>
                        <p class="text-sm text-gray-500">${item.size}</p>
                    </td>
                    <td class="py-3 px-2 sm:px-4">
                        <div class="flex items-center justify-center space-x-1 sm:space-x-2 no-print">
                            <button class="qty-btn decrease-qty" data-id="${item.id}">-</button>
                            <input type="number" value="${item.qty}" min="1" class="form-input w-12 sm:w-16 text-center p-1 qty-input" data-id="${item.id}">
                            <button class="qty-btn increase-qty" data-id="${item.id}">+</button>
                        </div>
                        <span class="print-only hidden">${item.qty}</span>
                    </td>
                    <td class="text-right py-3 px-2 sm:px-4">Rs ${item.price.toFixed(2)}</td>
                    <td class="text-right py-3 px-2 sm:px-4 font-semibold">Rs ${item.total.toFixed(2)}</td>
                    <td class="text-center py-3 px-2 sm:px-4 no-print">
                        <button class="remove-btn p-2 rounded-full text-red-500" data-id="${item.id}">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M6 18L18 6M6 6l12 12" /></svg>
                        </button>
                    </td>
                `;
                billingItems.appendChild(row);
            });
            attachRowEventListeners();
            updateTotals();
        }

        function attachRowEventListeners() {
            document.querySelectorAll('.increase-qty').forEach(b => b.onclick = e => updateItemQuantity(parseInt(e.currentTarget.dataset.id), items.find(i=>i.id===parseInt(e.currentTarget.dataset.id)).qty + 1));
            document.querySelectorAll('.decrease-qty').forEach(b => b.onclick = e => updateItemQuantity(parseInt(e.currentTarget.dataset.id), items.find(i=>i.id===parseInt(e.currentTarget.dataset.id)).qty - 1));
            document.querySelectorAll('.qty-input').forEach(i => i.onchange = e => updateItemQuantity(parseInt(e.currentTarget.dataset.id), parseFloat(e.currentTarget.value)));
            document.querySelectorAll('.remove-btn').forEach(b => b.onclick = e => {
                items = items.filter(item => item.id !== parseInt(e.currentTarget.dataset.id));
                renderTable();
            });
        }

        function updateTotals() {
            const subtotal = items.reduce((acc, item) => acc + item.total, 0);
            const discount = parseFloat(discountInput.value) || 0;
            const taxPercent = parseFloat(taxInput.value) || 0;
            const taxAmount = (subtotal - discount) * (taxPercent / 100);
            const total = subtotal - discount + taxAmount;

            subtotalEl.textContent = `Rs ${subtotal.toFixed(2)}`;
            totalEl.textContent = `Rs ${total.toFixed(2)}`;
        }

        function clearForm() {
            document.getElementById('productName').value = '';
            document.getElementById('productSize').value = '';
            document.getElementById('productQty').value = '1';
            document.getElementById('productPrice').value = '';
            document.getElementById('productName').focus();
        }

        function generatePDF() {
            const { jsPDF } = window.jspdf;
            const doc = new jsPDF();
            const invoiceNumber = invoiceNumberEl.value;
            const invoiceDate = invoiceDateEl.value;
            const clientName = document.getElementById('clientName').value || 'N/A';
            const clientContact = document.getElementById('clientContact').value || 'N/A';
            const notes = notesEl.value;

            const logoImg = new Image();
            logoImg.crossOrigin = 'Anonymous';
            logoImg.onload = function() {
                const canvas = document.createElement('canvas');
                canvas.width = this.naturalWidth;
                canvas.height = this.naturalHeight;
                canvas.getContext('2d').drawImage(this, 0, 0);
                const dataUrl = canvas.toDataURL('image/png');

                doc.addImage(dataUrl, 'PNG', 15, 12, 25, 25);
                doc.setFontSize(26); doc.setFont('helvetica', 'bold'); doc.text("K Cafe", 45, 25);
                doc.setFontSize(10); doc.setFont('helvetica', 'normal'); doc.text("Event Billing", 45, 31);
                doc.setFontSize(16); doc.setFont('helvetica', 'bold'); doc.text("INVOICE", 200, 20, { align: 'right' });
                doc.setFontSize(10); doc.text(`Invoice #: ${invoiceNumber}`, 200, 28, { align: 'right' });
                doc.text(`Date: ${invoiceDate}`, 200, 33, { align: 'right' });
                
                doc.setDrawColor(200); doc.line(15, 45, 200, 45);
                doc.setFontSize(11); doc.setFont('helvetica', 'bold'); doc.text("BILL TO", 15, 55);
                doc.setFont('helvetica', 'normal'); doc.text(clientName, 15, 61); doc.text(clientContact, 15, 66);
                
                const tableColumn = [
                    { header: 'Product', dataKey: 'name' },
                    { header: 'Qty', dataKey: 'qty' },
                    { header: 'Price', dataKey: 'price' },
                    { header: 'Total', dataKey: 'total' }
                ];
                const tableRows = items.map(item => ({
                    name: `${item.name}\n${item.size}`,
                    qty: item.qty,
                    price: `Rs ${item.price.toFixed(2)}`,
                    total: `Rs ${item.total.toFixed(2)}`
                }));

                doc.autoTable({
                    columns: tableColumn, body: tableRows, startY: 75, theme: 'grid',
                    headStyles: { fillColor: [107, 79, 75], textColor: 255, fontStyle: 'bold' },
                    styles: { cellPadding: 2.5, fontSize: 10, valign: 'middle' },
                    columnStyles: { qty: { halign: 'center' }, price: { halign: 'right' }, total: { halign: 'right' } }
                });
                
                let finalY = doc.lastAutoTable.finalY;

                const subtotal = items.reduce((acc, item) => acc + item.total, 0);
                const discount = parseFloat(discountInput.value) || 0;
                const taxPercent = parseFloat(taxInput.value) || 0;
                const taxAmount = (subtotal - discount) * (taxPercent / 100);
                const total = subtotal - discount + taxAmount;

                doc.setFontSize(10);
                let yPos = finalY + 10;
                doc.text(`Subtotal:`, 150, yPos, { align: 'right' }); doc.text(`Rs ${subtotal.toFixed(2)}`, 200, yPos, { align: 'right' });
                yPos += 6;
                doc.text(`Discount:`, 150, yPos, { align: 'right' }); doc.text(`- Rs ${discount.toFixed(2)}`, 200, yPos, { align: 'right' });
                yPos += 6;
                doc.text(`Tax (${taxPercent}%):`, 150, yPos, { align: 'right' }); doc.text(`+ Rs ${taxAmount.toFixed(2)}`, 200, yPos, { align: 'right' });
                yPos += 4;
                doc.setDrawColor(107, 79, 75); doc.line(130, yPos, 200, yPos);
                yPos += 6;
                doc.setFontSize(12); doc.setFont('helvetica', 'bold');
                doc.text(`Total:`, 150, yPos, { align: 'right' }); doc.text(`Rs ${total.toFixed(2)}`, 200, yPos, { align: 'right' });

                if (notes) {
                    doc.setFontSize(9);
                    doc.setFont('helvetica', 'bold');
                    doc.text("Terms & Conditions:", 15, finalY + 10);
                    doc.setFont('helvetica', 'normal');
                    const splitNotes = doc.splitTextToSize(notes, 100);
                    doc.text(splitNotes, 15, finalY + 16);
                }

                doc.setFontSize(9); doc.setTextColor(150);
                doc.text("Thank you for your business!", 105, doc.internal.pageSize.height - 10, { align: 'center' });

                doc.save(`KCafe-Invoice-${invoiceNumber}.pdf`);
            };
            logoImg.onerror = () => showModal("Could not load logo. PDF will be generated without it.");
            logoImg.src = 'https://placehold.co/200x200/f4f1eb/6b4f4b?text=K-Cafe';
        }

        addItemBtn.addEventListener('click', addItem);
        discountInput.addEventListener('input', updateTotals);
        taxInput.addEventListener('input', updateTotals);
        generatePdfBtn.addEventListener('click', generatePDF);

        setInitialValues();
        renderTable();
    });
    </script>
</body>
</html>
