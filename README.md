<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DivoraSplit Dashboard</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            background: #f4f4f4;
        }
        header {
            background: #333;
            color: white;
            padding: 10px 20px;
            display: flex;
            justify-content: space-between;
            align-items: center;
            position: fixed;
            width: 100%;
            top: 0;
            z-index: 100;
            pointer-events: auto;
        }
        #hamburger {
            cursor: pointer;
            font-size: 24px;
            z-index: 101;
            pointer-events: auto;
        }
        #menu {
            display: none;
            position: absolute;
            top: 60px;
            left: 10px;
            background: white;
            border: 1px solid #ddd;
            padding: 10px;
            z-index: 50;
            box-shadow: 0 2px 5px rgba(0,0,0,0.2);
        }
        #menu ul {
            list-style: none;
            padding: 0;
            margin: 0;
        }
        #menu li {
            padding: 10px;
            cursor: pointer;
        }
        #menu li:hover {
            background: #f0f0f0;
        }
        #main-content {
            max-width: 800px;
            margin: 80px auto 20px;
            padding: 20px;
        }
        section {
            display: none;
            background: white;
            padding: 20px;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        #dashboard-section {
            display: block;
        }
        h1, h2 {
            color: #333;
        }
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }
        select, input {
            width: 100%;
            padding: 8px;
            margin-bottom: 10px;
            box-sizing: border-box;
            border: 1px solid #ddd;
            border-radius: 4px;
        }
        button {
            padding: 10px 20px;
            cursor: pointer;
            background: #007bff;
            color: white;
            border: none;
            border-radius: 4px;
            z-index: 10;
            pointer-events: auto;
        }
        button:hover {
            background: #0056b3;
        }
        @media (max-width: 600px) {
            #main-content {
                margin: 60px 10px 10px;
                padding: 10px;
            }
            button {
                width: 100%;
                margin-bottom: 10px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div id="hamburger">&#9776;</div>
        <h1>DivoraSplit Dashboard</h1>
    </header>
    <nav id="menu">
        <ul>
            <li data-section="dashboard-section">Dashboard</li>
            <li data-section="co-funding-section">Co-Funding Request</li>
        </ul>
    </nav>
    <div id="main-content">
        <section id="dashboard-section">
            <h1>Dashboard</h1>
            <p>Welcome to DivoraSplit! Start by submitting a co-funding request.</p>
        </section>
        <section id="co-funding-section">
            <h1>Co-Funding Request</h1>
            <label for="prop-firm">Select Prop Firm:</label>
            <select id="prop-firm">
                <option value="FTMO">FTMO</option>
                <option value="The5ers">The5ers</option>
            </select>
            <label for="account-size">Account Size:</label>
            <select id="account-size">
                <option value="">Select a firm first</option>
            </select>
            <label for="profit-split">Profit Split (Requester:Co-Funder):</label>
            <input type="text" id="profit-split" placeholder="e.g., 50:50">
            <button id="submit-request">Submit Request</button>
            <h2>Recent Requests</h2>
            <ul id="recent-requests"></ul>
        </section>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            console.log('DOM loaded, initializing DivoraSplit Dashboard...');
    // Prop Firm Data
            const propFirms = {
                "FTMO": [
                    { size: "$10,000", price: 155.0 },
                    { size: "$100,000", price: 500.0 }
                ],
                "The5ers": [
                    { size: "$10,000", price: 145.0 },
                    { size: "$100,000", price: 480.0 }
                ]
            };

   // DOM Elements
            const hamburger = document.getElementById('hamburger');
            const menu = document.getElementById('menu');
            const menuItems = menu ? menu.querySelectorAll('li') : [];
            const sections = document.querySelectorAll('section');
            const firmSelect = document.getElementById('prop-firm');
            const accountSize = document.getElementById('account-size');
            const profitSplitInput = document.getElementById('profit-split');
            const submitButton = document.getElementById('submit-request');
            const recentRequests = document.getElementById('recent-requests');

   // State
            let selectedAccount = '';
            let accountPrice = 0.0;
            const userId = 'user123'; // Simulated user ID

   // Debug all button clicks
            document.querySelectorAll('button').forEach(btn => {
                btn.addEventListener('click', () => {
 console.log(`Button clicked: ${btn.id || btn.textContent}`);
                });
            });

 // Hamburger Menu
            console.log('Checking hamburger and menu elements...');
            if (hamburger) {
                console.log('Hamburger element found:', hamburger);
                hamburger.addEventListener('click', () => {
                    console.log('Hamburger menu clicked');
                    if (menu) {
                        console.log('Menu element found, current display:', menu.style.display);
                        menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                        console.log('Menu display set to:', menu.style.display);
                    } else {
                        console.error('Menu element not found');
                    }
                });
            } else {
                console.error('Hamburger element not found');
            }

  // Menu Navigation
            if (menuItems.length > 0) {
                console.log(`Found ${menuItems.length} menu items`);
                menuItems.forEach(item => {
                    item.addEventListener('click', () => {
                        const sectionId = item.dataset.section;
                        console.log(`Menu item clicked: ${sectionId}`);
                        sections.forEach(sec => {
                            sec.style.display = 'none';
                            console.log(`Hiding section: ${sec.id}`);
                        });
                        const targetSection = document.getElementById(sectionId);
                        if (targetSection) {
                            targetSection.style.display = 'block';
                            console.log(`Showing section: ${sectionId}`);
                        } else {
                            console.error(`Section not found: ${sectionId}`);
                            document.getElementById('dashboard-section').style.display = 'block';
                            console.log('Fallback: Showing dashboard-section');
                        }
                        if (menu) {
                            menu.style.display = 'none';
                            console.log('Menu hidden after section selection');
                        }
                    });
                });
            } else {
                console.error('No menu items found');
            }
  // Data Management
            function loadRequests() {
                try {
                    return JSON.parse(localStorage.getItem('requests')) || [];
                } catch (e) {
                    console.error('Error loading requests:', e);
                    return [];
                }
            }

  function saveRequests(requests) {
                try {
                    localStorage.setItem('requests', JSON.stringify(requests));
                } catch (e) {
                    console.error('Error saving requests:', e);
                    alert('Error saving request. Check console.');
                }
            }

  // Co-Funding Functions
            function updateAccountSizes() {
                console.log('Updating account sizes for firm:', firmSelect.value);
                const firm = firmSelect.value;
                accountSize.innerHTML = '<option value="">Select an account size</option>';
                selectedAccount = '';
                accountPrice = 0.0;

  if (firm && propFirms[firm]) {
                    propFirms[firm].forEach(acc => {
                        const option = document.createElement('option');
                        option.value = acc.size;
                        option.textContent = `${acc.size} - $${acc.price.toFixed(2)}`;
                        accountSize.appendChild(option);
                    });
                }
            }

  function submitRequest() {
                console.log('Submit Co-Funding Request clicked');
                const firm = firmSelect.value;
                const split = profitSplitInput.value || '50:50';

 if (!firm || !propFirms[firm]) {
                    alert('Please select a valid Prop Firm.');
                    return;
                }
                if (!selectedAccount) {
                    alert('Please select an account size.');
                    return;
                }
                if (!split.match(/^\d+:\d+$/)) {
                    alert('Please enter a valid profit split (e.g., 50:50).');
                    return;
                }

 const [reqPart, coPart] = split.split(':').map(Number);
                const totalParts = reqPart + coPart;
                if (totalParts === 0) {
                    alert('Invalid profit split ratio.');
                    return;
                }
                const requesterShare = (reqPart / totalParts) * accountPrice;
                const coFunderShare = (coPart / totalParts) * accountPrice;

  const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'co-funding',
                    prop_firm: firm,
                    account_size: selectedAccount,
                    account_price: accountPrice,
                    profit_split: split,
                    requester_share: requesterShare,
                    co_funder_share: coFunderShare,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);

   alert(`Co-Funding Request submitted! Your share: $${requesterShare.toFixed(2)}. Status: Pending`);
                profitSplitInput.value = '50:50';
                firmSelect.value = 'FTMO';
                updateAccountSizes();
                loadRecentRequests();
            }

  function loadRecentRequests() {
                console.log('Loading recent requests');
                recentRequests.innerHTML = '';
                const requests = loadRequests().filter(req => req.type === 'co-funding' && req.user_id === userId).slice(-3);
                if (requests.length === 0) {
                    recentRequests.innerHTML = '<li>No recent requests.</li>';
                    return;
                }
                requests.forEach(req => {
                    const li = document.createElement('li');
                    li.textContent = `ID: ${req.id} | Firm: ${req.prop_firm} | Size: ${req.account_size} | Your Share: $${req.requester_share.toFixed(2)} | Status: ${req.status} | ${req.timestamp}`;
                    recentRequests.appendChild(li);
                });
            }

  // Event Listeners
            if (firmSelect) {
                firmSelect.addEventListener('change', updateAccountSizes);
            }
            if (accountSize) {
                accountSize.addEventListener('change', () => {
                    console.log('Account size selected:', accountSize.value);
                    selectedAccount = accountSize.value;
                    const firm = firmSelect.value;
                    const acc = propFirms[firm]?.find(a => a.size === selectedAccount);
                    accountPrice = acc ? acc.price : 0.0;
                });
            }
            if (submitButton) {
                submitButton.addEventListener('click', submitRequest);
            }

  // Initial Load
            console.log('Initializing dashboard');
            updateAccountSizes();
            loadRecentRequests();
        });
    </script>
</body>
</html>
