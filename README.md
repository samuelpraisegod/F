<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DivoraSplit Dashboard</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        body {
            font-family: Arial, sans-serif;
            background: #f4f4f4;
        }
        header {
            background: #222;
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
            color: white;
        }
        #sidebar {
            position: fixed;
            top: 60px;
            left: -250px;
            width: 250px;
            height: calc(100vh - 60px);
            background: #222;
            color: #ccc;
            transition: left 0.3s ease;
            z-index: 99;
            overflow-y: auto;
        }
        #sidebar.active {
            left: 0;
        }
        #sidebar ul {
            list-style: none;
            padding: 0;
        }
        #sidebar li {
            padding: 15px 20px;
            cursor: pointer;
            display: flex;
            align-items: center;
            font-size: 16px;
        }
        #sidebar li:hover, #sidebar li.active {
            background: #007bff;
            color: white;
        }
        #sidebar .icon {
            margin-right: 10px;
            font-size: 20px;
        }
        #sidebar .sub-menu {
            display: none;
            padding-left: 30px;
            background: #333;
        }
        #sidebar .sub-menu li {
            padding: 10px 20px;
            font-size: 14px;
        }
        #sidebar .sub-menu li:hover, #sidebar .sub-menu li.active {
            background: #0056b3;
        }
        #main-content {
            margin: 80px 20px 20px 20px;
            max-width: 800px;
            padding: 20px;
            background: white;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.1);
        }
        @media (min-width: 768px) {
            #main-content {
                margin-left: 280px;
            }
            #sidebar.active + #main-content {
                margin-left: 280px;
            }
        }
        section {
            display: none;
        }
        section.active {
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
        ul#recent-requests {
            padding-left: 20px;
        }
        @media (max-width: 767px) {
            #main-content {
                margin: 80px 10px 10px;
            }
            #sidebar {
                width: 200px;
                left: -200px;
            }
            #sidebar.active {
                left: 0;
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
    <nav id="sidebar">
        <ul>
            <li data-section="dashboard-section"><span class="icon">🏠</span> Dashboard</li>
            <li class="has-sub-menu"><span class="icon">👤</span> Profile
                <ul class="sub-menu">
                    <li data-section="profile-view-section">View Profile</li>
                    <li data-section="profile-edit-section">Edit Profile</li>
                    <li data-section="settings-section">Settings</li>
                    <li data-section="logout-section">Logout</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">🏦</span> Balance
                <ul class="sub-menu">
                    <li data-section="wallet-overview-section">Wallet Overview</li>
                    <li data-section="transaction-history-section">Transaction History</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">➕</span> Deposit
                <ul class="sub-menu">
                    <li data-section="deposit-bank-section">Bank Transfer</li>
                    <li data-section="deposit-crypto-section">Crypto Wallet</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">💵</span> Withdrawals
                <ul class="sub-menu">
                    <li data-section="withdraw-bank-section">Bank Withdrawal</li>
                    <li data-section="withdraw-crypto-section">Crypto Withdrawal</li>
                    <li data-section="withdraw-history-section">Withdrawal History</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">🤝</span> Co-Funding
                <ul class="sub-menu">
                    <li data-section="co-funding-section">Create Co-Funding Request</li>
                    <li data-section="pending-requests-section">Pending Requests</li>
                    <li data-section="active-agreements-section">Active Agreements</li>
                    <li data-section="completed-agreements-section">Completed Agreements</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">💼</span> Available Funded
                <ul class="sub-menu">
                    <li data-section="my-funded-section">My Funded Accounts</li>
                    <li data-section="partnered-accounts-section">Partnered Accounts</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">👥</span> Available Traders
                <ul class="sub-menu">
                    <li data-section="browse-traders-section">Browse Traders</li>
                    <li data-section="trader-profiles-section">Trader Profiles</li>
                    <li data-section="apply-partnership-section">Apply for Partnership</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">📩</span> Requests
                <ul class="sub-menu">
                    <li data-section="requests-co-funding-section">Co-Funding Requests</li>
                    <li data-section="managed-trading-requests-section">Managed Trading Requests</li>
                    <li data-section="managed-account-requests-section">Managed Account Requests</li>
                    <li data-section="requests-dashboard-section">Dashboard (All Requests)</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">📑</span> Orders
                <ul class="sub-menu">
                    <li data-section="open-orders-section">Open Orders</li>
                    <li data-section="order-history-section">Order History</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">✅</span> Completed
                <ul class="sub-menu">
                    <li data-section="completed-trades-section">Completed Trades</li>
                    <li data-section="completed-requests-section">Completed Requests</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">📈</span> Performance Chart
                <ul class="sub-menu">
                    <li data-section="account-performance-section">Account Performance</li>
                    <li data-section="profit-loss-section">Profit/Loss Analytics</li>
                </ul>
            </li>
            <li class="has-sub-menu"><span class="icon">💬</span> Messages
                <ul class="sub-menu">
                    <li data-section="inbox-section">Inbox</li>
                    <li data-section="sent-messages-section">Sent Messages</li>
                    <li data-section="notifications-section">Notifications</li>
                </ul>
            </li>
        </ul>
    </nav>
    <div id="main-content">
        <section id="dashboard-section" class="active">
            <h1>Dashboard</h1>
            <p>Welcome to DivoraSplit! Navigate using the sidebar to manage your account.</p>
        </section>
        <section id="co-funding-section">
            <h1>Create Co-Funding Request</h1>
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
        <!-- Placeholder sections for other menu items -->
        <section id="profile-view-section"><h1>View Profile</h1><p>Profile details here.</p></section>
        <section id="profile-edit-section"><h1>Edit Profile</h1><p>Edit profile form here.</p></section>
        <section id="settings-section"><h1>Settings</h1><p>Settings options here.</p></section>
        <section id="logout-section"><h1>Logout</h1><p>Logout confirmation here.</p></section>
        <section id="wallet-overview-section"><h1>Wallet Overview</h1><p>Balance details here.</p></section>
        <section id="transaction-history-section"><h1>Transaction History</h1><p>Transaction list here.</p></section>
        <section id="deposit-bank-section"><h1>Bank Transfer</h1><p>Bank deposit form here.</p></section>
        <section id="deposit-crypto-section"><h1>Crypto Wallet</h1><p>Crypto deposit form here.</p></section>
        <section id="withdraw-bank-section"><h1>Bank Withdrawal</h1><p>Bank withdrawal form here.</p></section>
        <section id="withdraw-crypto-section"><h1>Crypto Withdrawal</h1><p>Crypto withdrawal form here.</p></section>
        <section id="withdraw-history-section"><h1>Withdrawal History</h1><p>Withdrawal list here.</p></section>
        <section id="pending-requests-section"><h1>Pending Requests</h1><p>Pending co-funding requests here.</p></section>
        <section id="active-agreements-section"><h1>Active Agreements</h1><p>Active agreements list here.</p></section>
        <section id="completed-agreements-section"><h1>Completed Agreements</h1><p>Completed agreements list here.</p></section>
        <section id="my-funded-section"><h1>My Funded Accounts</h1><p>Funded accounts list here.</p></section>
        <section id="partnered-accounts-section"><h1>Partnered Accounts</h1><p>Partnered accounts list here.</p></section>
        <section id="browse-traders-section"><h1>Browse Traders</h1><p>Trader list here.</p></section>
        <section id="trader-profiles-section"><h1>Trader Profiles</h1><p>Trader profiles here.</p></section>
        <section id="apply-partnership-section"><h1>Apply for Partnership</h1><p>Partnership application here.</p></section>
        <section id="requests-co-funding-section"><h1>Co-Funding Requests</h1><p>Co-funding requests list here.</p></section>
        <section id="managed-trading-requests-section"><h1>Managed Trading Requests</h1><p>Managed trading requests list here.</p></section>
        <section id="managed-account-requests-section"><h1>Managed Account Requests</h1><p>Managed account requests list here.</p></section>
        <section id="requests-dashboard-section"><h1>Requests Dashboard</h1><p>All requests overview here.</p></section>
        <section id="open-orders-section"><h1>Open Orders</h1><p>Open orders list here.</p></section>
        <section id="order-history-section"><h1>Order History</h1><p>Order history list here.</p></section>
        <section id="completed-trades-section"><h1>Completed Trades</h1><p>Completed trades list here.</p></section>
        <section id="completed-requests-section"><h1>Completed Requests</h1><p>Completed requests list here.</p></section>
        <section id="account-performance-section"><h1>Account Performance</h1><p>Performance chart here.</p></section>
        <section id="profit-loss-section"><h1>Profit/Loss Analytics</h1><p>Profit/loss analytics here.</p></section>
        <section id="inbox-section"><h1>Inbox</h1><p>Messages inbox here.</p></section>
        <section id="sent-messages-section"><h1>Sent Messages</h1><p>Sent messages list here.</p></section>
        <section id="notifications-section"><h1>Notifications</h1><p>Notifications list here.</p></section>
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
            const sidebar = document.getElementById('sidebar');
            const menuItems = sidebar ? sidebar.querySelectorAll('li:not(.sub-menu li)') : [];
            const subMenuItems = sidebar ? sidebar.querySelectorAll('.sub-menu li') : [];
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

   // Debug Button Clicks
            document.querySelectorAll('button').forEach(btn => {
                btn.addEventListener('click', () => {
                    console.log(`Button clicked: ${btn.id || btn.textContent}`);
                });
            });

  // Hamburger Menu
            if (hamburger) {
                console.log('Hamburger element found:', hamburger);
                hamburger.addEventListener('click', () => {
                    console.log('Hamburger menu clicked');
                    if (sidebar) {
                        sidebar.classList.toggle('active');
                        console.log('Sidebar display set to:', sidebar.classList.contains('active') ? 'block' : 'none');
                    } else {
                        console.error('Sidebar element not found');
                    }
                });
            } else {
                console.error('Hamburger element not found');
            }

  // Accordion Menu
            menuItems.forEach(item => {
                if (item.classList.contains('has-sub-menu')) {
                    item.addEventListener('click', (e) => {
                        e.stopPropagation();
                        console.log(`Main menu item clicked: ${item.textContent.trim()}`);
                        const subMenu = item.querySelector('.sub-menu');
                        if (subMenu) {
                            const isDisplayed = subMenu.style.display === 'block';
                            subMenu.style.display = isDisplayed ? 'none' : 'block';
                            console.log(`Sub-menu display set to: ${subMenu.style.display}`);
                        }
                    });
                }
            });

 // Sub-Menu Navigation
            console.log(`Found ${subMenuItems.length} sub-menu items`);
            subMenuItems.forEach(item => {
                item.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const sectionId = item.dataset.section;
                    console.log(`Menu item clicked: ${sectionId}`);
                    sections.forEach(sec => {
                        sec.classList.remove('active');
                        console.log(`Hiding section: ${sec.id}`);
                    });
                    const targetSection = document.getElementById(sectionId);
                    if (targetSection) {
                        targetSection.classList.add('active');
                        console.log(`Showing section: ${sectionId}`);
                    } else {
                        console.error(`Section not found: ${sectionId}`);
                        document.getElementById('dashboard-section').classList.add('active');
                        console.log('Fallback: Showing dashboard-section');
                    }
                    subMenuItems.forEach(i => i.classList.remove('active'));
                    menuItems.forEach(i => i.classList.remove('active'));
                    item.classList.add('active');
                    item.closest('.has-sub-menu').classList.add('active');
                    if (window.innerWidth <= 767) {
                        sidebar.classList.remove('active');
                        console.log('Sidebar hidden on mobile after selection');
                    }
                });
            });

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
