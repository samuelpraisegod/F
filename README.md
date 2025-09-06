<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DivoraSplit Dashboard - Balance</title>
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
            margin-bottom: 20px;
        }
        .balance-overview {
            background: #e9ecef;
            padding: 15px;
            border-radius: 4px;
            margin-bottom: 20px;
        }
        .balance-overview p {
            margin: 5px 0;
            font-size: 16px;
        }
        .action-buttons {
            display: flex;
            gap: 10px;
            margin-bottom: 20px;
        }
        button {
            padding: 10px 20px;
            cursor: pointer;
            background: #28a745;
            color: white;
            border: none;
            border-radius: 4px;
            font-size: 16px;
            pointer-events: auto;
        }
        button:hover {
            background: #218838;
        }
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
        }
        th, td {
            padding: 10px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        th {
            background: #f8f9fa;
        }
        /* Deposit and Withdrawal styles */
        .tabs {
            display: flex;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }
        .tab {
            padding: 10px 20px;
            cursor: pointer;
            background: #ddd;
            margin-right: 5px;
            border-radius: 4px 4px 0 0;
            font-size: 16px;
        }
        .tab.active {
            background: #007bff;
            color: white;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
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
        .instructions {
            background: #e9ecef;
            padding: 10px;
            border-radius: 4px;
            margin: 10px 0;
            font-size: 14px;
        }
        .copy-button {
            background: #6c757d;
            margin-left: 10px;
        }
        .copy-button:hover {
            background: #5a6268;
        }
        .qr-code {
            width: 100px;
            height: 100px;
            background: #ddd;
            margin: 10px 0;
            border: 1px solid #ccc;
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
            .action-buttons {
                flex-direction: column;
            }
            button {
                width: 100%;
            }
            table {
                font-size: 14px;
            }
            th, td {
                padding: 8px;
            }
            .tab {
                flex: 1 1 100%;
                margin-bottom: 5px;
                text-align: center;
            }
        }
    </style>
</head>
<body>
    <header>
        <div id="hamburger">&#9776;</div>
        <h1>DivoraSplit Dashboard 🌐</h1>
    </header>
    <nav id="sidebar">
        <ul>
            <li data-section="dashboard-section"><span class="icon">🏠</span> Dashboard</li>
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
                </ul>
            </li>
        </ul>
    </nav>
    <div id="main-content">
        <section id="wallet-overview-section" class="active">
            <h1>Balance Dashboard</h1>
            <div class="balance-overview">
                <p><strong>Current Balance:</strong> $2,350.00</p>
                <p><strong>Available Balance:</strong> $2,100.00</p>
                <p><strong>Pending Balance:</strong> $250.00</p>
            </div>
            <div class="action-buttons">
                <button id="deposit-button">➕ Deposit Funds</button>
                <button id="withdraw-button">💵 Withdraw Funds</button>
            </div>
            <h2>Transaction History</h2>
            <table>
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Type</th>
                        <th>Amount</th>
                        <th>Status</th>
                    </tr>
                </thead>
                <tbody id="transaction-history"></tbody>
            </table>
        </section>
        <section id="deposit-bank-section">
            <h1>Deposit Funds</h1>
            <div class="tabs">
                <div class="tab active" data-tab="bank-tab">🏦 Bank Transfer</div>
                <div class="tab" data-tab="crypto-tab">🔗 Crypto Wallet</div>
            </div>
            <div id="bank-tab" class="tab-content active">
                <label for="bank-amount">Amount ($):</label>
                <input type="number" id="bank-amount" value="500" min="1">
                <label for="bank-currency">Currency:</label>
                <select id="bank-currency">
                    <option value="USD">USD</option>
                    <option value="NGN">NGN</option>
                    <option value="EUR">EUR</option>
                </select>
                <p><strong>Bank Name:</strong> GTBank</p>
                <p><strong>Account Name:</strong> MarketFlowFX Ltd</p>
                <p><strong>Account Number:</strong> 0123456789</p>
                <p><strong>Reference Code:</strong> <span id="bank-reference">DEP-XXXXX</span></p>
                <label for="bank-proof">Upload Proof of Payment (Optional):</label>
                <input type="file" id="bank-proof" accept="image/*,application/pdf">
                <div class="instructions">Bank transfers take 1–2 business days.</div>
            </div>
            <div id="crypto-tab" class="tab-content">
                <label for="crypto-amount">Amount ($):</label>
                <input type="number" id="crypto-amount" value="500" min="1">
                <label for="crypto-currency">Currency:</label>
                <select id="crypto-currency">
                    <option value="USD">USD</option>
                    <option value="NGN">NGN</option>
                    <option value="EUR">EUR</option>
                </select>
                <label for="coin-type">Select Coin:</label>
                <select id="coin-type">
                    <option value="BTC">BTC</option>
                    <option value="USDT">USDT</option>
                    <option value="ETH">ETH</option>
                </select>
                <p><strong>Wallet Address:</strong> 0xAB123456789CDEFFED1234... <button class="copy-button" onclick="navigator.clipboard.writeText('0xAB123456789CDEFFED1234...').then(() => alert('Address copied!'))">📋 Copy</button></p>
                <p><strong>QR Code:</strong></p>
                <div class="qr-code"></div>
                <label for="crypto-proof">Upload Proof of Payment (Optional):</label>
                <input type="file" id="crypto-proof" accept="image/*,application/pdf">
                <div class="instructions">Crypto deposits take up to 1 hour.</div>
            </div>
            <button id="confirm-deposit">✅ Confirm Deposit</button>
        </section>
        <section id="withdraw-bank-section">
            <h1>Withdraw Funds</h1>
            <div class="balance-overview">
                <p><strong>Available Balance:</strong> $2,100.00</p>
                <p><strong>Pending Withdrawals:</strong> $250.00</p>
                <p><strong>Minimum Withdrawal:</strong> $10.00</p>
            </div>
            <label for="withdraw-amount">Enter Withdrawal Amount ($):</label>
            <input type="number" id="withdraw-amount" value="200" min="10" step="0.01">
            <label for="withdraw-currency">Currency:</label>
            <select id="withdraw-currency">
                <option value="USD">USD</option>
                <option value="NGN">NGN</option>
                <option value="EUR">EUR</option>
            </select>
            <p id="fee-preview">You will receive: Calculating...</p>
            <div class="tabs">
                <div class="tab active" data-tab="bank-tab">🏦 Bank Transfer</div>
                <div class="tab" data-tab="crypto-tab">🔗 Crypto Wallet</div>
            </div>
            <div id="bank-tab" class="tab-content active">
                <label for="bank-name">Bank Name:</label>
                <select id="bank-name">
                    <option value="GTBank">GTBank</option>
                    <option value="Zenith Bank">Zenith Bank</option>
                    <option value="First Bank">First Bank</option>
                </select>
                <label for="bank-account-number">Account Number:</label>
                <input type="text" id="bank-account-number" placeholder="e.g., 0123456789">
                <label for="bank-account-name">Account Holder Name:</label>
                <input type="text" id="bank-account-name" placeholder="e.g., John Doe">
                <label for="bank-branch">Branch (Optional):</label>
                <input type="text" id="bank-branch" placeholder="e.g., Lagos Main">
                <label><input type="checkbox" id="bank-default"> Save as Default Payout Account</label>
                <div class="instructions">Bank transfers take 1–2 business days.</div>
            </div>
            <div id="crypto-tab" class="tab-content">
                <label for="coin-type">Select Coin:</label>
                <select id="coin-type">
                    <option value="BTC">BTC</option>
                    <option value="USDT">USDT</option>
                    <option value="ETH">ETH</option>
                </select>
                <label for="crypto-network">Network Type:</label>
                <select id="crypto-network">
                    <option value="ERC20">ERC20</option>
                    <option value="TRC20">TRC20</option>
                    <option value="BEP20">BEP20</option>
                </select>
                <label for="crypto-wallet">Wallet Address:</label>
                <input type="text" id="crypto-wallet" placeholder="e.g., 0xAB123456789...">
                <button class="copy-button" onclick="navigator.clipboard.readText().then(text => document.getElementById('crypto-wallet').value = text).then(() => alert('Pasted from clipboard!'))">📋 Paste</button>
                <div class="instructions">Crypto withdrawals take up to 1 hour.</div>
            </div>
            <label for="otp-code">Enter OTP/2FA Code:</label>
            <input type="text" id="otp-code" placeholder="e.g., 123456">
            <button id="submit-withdrawal">✅ Submit Withdrawal</button>
        </section>
        <section id="withdraw-crypto-section">
            <h1>Withdraw Funds (Crypto)</h1>
            <p>Redirecting to Crypto Wallet withdrawal...</p>
        </section>
        <section id="withdraw-history-section">
            <h1>Withdrawal History</h1>
            <p>Full withdrawal history here.</p>
        </section>
        <section id="transaction-history-section">
            <h1>Transaction History</h1>
            <p>Full transaction history here.</p>
        </section>
        <section id="dashboard-section">
            <h1>Dashboard</h1>
            <p>Welcome to DivoraSplit! Navigate using the sidebar to manage your account.</p>
        </section>
        <section id="co-funding-section"><h1>Create Co-Funding Request</h1><p>Co-funding form here.</p></section>
        <section id="pending-requests-section"><h1>Pending Requests</h1><p>Pending co-funding requests here.</p></section>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            console.log('DOM loaded, initializing DivoraSplit Dashboard...');

            // DOM Elements
            const hamburger = document.getElementById('hamburger');
            const sidebar = document.getElementById('sidebar');
            const menuItems = sidebar ? sidebar.querySelectorAll('li:not(.sub-menu li)') : [];
            const subMenuItems = sidebar ? sidebar.querySelectorAll('.sub-menu li') : [];
            const sections = document.querySelectorAll('section');
            const depositButton = document.getElementById('deposit-button');
            const withdrawButton = document.getElementById('withdraw-button');
            const transactionHistory = document.getElementById('transaction-history');
            const confirmDeposit = document.getElementById('confirm-deposit');
            const submitWithdrawal = document.getElementById('submit-withdrawal');

            // State
            const userId = 'user123';
            const currentBalance = 2350.00;
            const availableBalance = 2100.00;
            const pendingBalance = 250.00;
            const minWithdrawal = 10.00;
            const feeRate = 0.025; // 2.5% fee
            let selectedDepositMethod = 'Bank Transfer';
            let selectedWithdrawMethod = 'Bank Transfer';

            // Debug Button Clicks
            document.querySelectorAll('button').forEach(btn => {
                btn.addEventListener('click', () => {
                    console.log(`Button clicked: ${btn.id || btn.textContent}`);
                });
            });

            // Hamburger Menu Fix
            if (!hamburger) {
                console.error('Hamburger element not found. Ensure <div id="hamburger"> exists in <header>.');
            } else {
                console.log('Hamburger element found:', hamburger);
                hamburger.addEventListener('click', () => {
                    if (!sidebar) {
                        console.error('Sidebar element not found. Ensure <nav id="sidebar"> exists.');
                        return;
                    }
                    console.log('Hamburger menu clicked');
                    sidebar.classList.toggle('active');
                    console.log('Sidebar state:', sidebar.classList.contains('active') ? 'Visible (left: 0)' : 'Hidden (left: -250px)');
                });
            }

            // Accordion Menu
            console.log(`Found ${menuItems.length} menu items`);
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
                        document.getElementById('wallet-overview-section').classList.add('active');
                        console.log('Fallback: Showing wallet-overview-section');
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

            // Quick Action Buttons
            if (depositButton) {
                depositButton.addEventListener('click', () => {
                    console.log('Deposit button clicked');
                    sections.forEach(sec => sec.classList.remove('active'));
                    document.getElementById('deposit-bank-section').classList.add('active');
                    console.log('Showing section: deposit-bank-section');
                    if (window.innerWidth <= 767) {
                        sidebar.classList.remove('active');
                    }
                });
            }
            if (withdrawButton) {
                withdrawButton.addEventListener('click', () => {
                    console.log('Withdraw button clicked');
                    sections.forEach(sec => sec.classList.remove('active'));
                    document.getElementById('withdraw-bank-section').classList.add('active');
                    console.log('Showing section: withdraw-bank-section');
                    if (window.innerWidth <= 767) {
                        sidebar.classList.remove('active');
                    }
                });
            }

            // Deposit and Withdrawal Tabs
            const depositTabs = document.querySelectorAll('#deposit-bank-section .tab');
            const depositTabContents = document.querySelectorAll('#deposit-bank-section .tab-content');
            const withdrawTabs = document.querySelectorAll('#withdraw-bank-section .tab');
            const withdrawTabContents = document.querySelectorAll('#withdraw-bank-section .tab-content');

            depositTabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    console.log(`Deposit tab clicked: ${tab.dataset.tab}`);
                    depositTabs.forEach(t => t.classList.remove('active'));
                    depositTabContents.forEach(c => c.classList.remove('active'));
                    tab.classList.add('active');
                    document.getElementById(tab.dataset.tab).classList.add('active');
                    selectedDepositMethod = tab.textContent.trim();
                    console.log(`Selected deposit method: ${selectedDepositMethod}`);
                    updateReferenceCode();
                });
            });

            withdrawTabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    console.log(`Withdraw tab clicked: ${tab.dataset.tab}`);
                    withdrawTabs.forEach(t => t.classList.remove('active'));
                    withdrawTabContents.forEach(c => c.classList.remove('active'));
                    tab.classList.add('active');
                    document.getElementById(tab.dataset.tab).classList.add('active');
                    selectedWithdrawMethod = tab.textContent.trim();
                    console.log(`Selected withdraw method: ${selectedWithdrawMethod}`);
                    updateFeePreview();
                });
            });

            // Data Management
            function loadTransactions() {
                try {
                    const deposits = JSON.parse(localStorage.getItem('deposits')) || [];
                    const withdrawals = JSON.parse(localStorage.getItem('withdrawals')) || [];
                    return [
                        ...deposits.map(d => ({ ...d, type: 'Deposit' })),
                        ...withdrawals.map(w => ({ ...w, type: 'Withdrawal' }))
                    ].filter(t => t.user_id === userId).sort((a, b) => new Date(b.timestamp) - new Date(a.timestamp));
                } catch (e) {
                    console.error('Error loading transactions:', e);
                    return [];
                }
            }

            function saveDeposits(deposits) {
                try {
                    localStorage.setItem('deposits', JSON.stringify(deposits));
                } catch (e) {
                    console.error('Error saving deposits:', e);
                    alert('Error saving deposit. Check console.');
                }
            }

            function saveWithdrawals(withdrawals) {
                try {
                    localStorage.setItem('withdrawals', JSON.stringify(withdrawals));
                } catch (e) {
                    console.error('Error saving withdrawals:', e);
                    alert('Error saving withdrawal. Check console.');
                }
            }

            function loadTransactionHistory() {
                console.log('Loading transaction history');
                transactionHistory.innerHTML = '';
                const transactions = loadTransactions().slice(0, 5);
                if (transactions.length === 0) {
                    transactionHistory.innerHTML = '<tr><td colspan="4">No recent transactions.</td></tr>';
                    return;
                }
                transactions.forEach(t => {
                    const tr = document.createElement('tr');
                    const statusIcon = t.status === 'Approved' ? '✅' : t.status === 'Pending' ? '⏳' : '❌';
                    tr.innerHTML = `
                        <td>${t.timestamp.split(',')[0]}</td>
                        <td>${t.type}</td>
                        <td>$${t.amount.toFixed(2)}</td>
                        <td>${statusIcon} ${t.status}</td>
                    `;
                    transactionHistory.appendChild(tr);
                });
            }

            // Deposit Functions
            function generateReferenceCode() {
                return 'DEP-' + Math.random().toString(36).substr(2, 5).toUpperCase();
            }

            function updateReferenceCode() {
                const bankReference = document.getElementById('bank-reference');
                if (bankReference && selectedDepositMethod === 'Bank Transfer') {
                    bankReference.textContent = generateReferenceCode();
                }
            }

            function submitDeposit() {
                console.log(`Confirm Deposit clicked, method: ${selectedDepositMethod}`);
                let amount, currency, details = {};

                if (selectedDepositMethod === 'Bank Transfer') {
                    amount = parseFloat(document.getElementById('bank-amount').value);
                    currency = document.getElementById('bank-currency').value;
                    const proof = document.getElementById('bank-proof')?.files[0];
                    details.proof = proof ? proof.name : 'No proof uploaded';
                    details.bank_name = 'GTBank';
                    details.account_name = 'MarketFlowFX Ltd';
                    details.account_number = '0123456789';
                    details.reference_code = document.getElementById('bank-reference').textContent;
                } else if (selectedDepositMethod === 'Crypto Wallet') {
                    amount = parseFloat(document.getElementById('crypto-amount').value);
                    currency = document.getElementById('crypto-currency').value;
                    const coinType = document.getElementById('coin-type').value;
                    const proof = document.getElementById('crypto-proof')?.files[0];
                    details.coin_type = coinType;
                    details.wallet_address = '0xAB123456789CDEFFED1234...';
                    details.proof = proof ? proof.name : 'No proof uploaded';
                }

                if (!amount || amount <= 0) {
                    alert('Please enter a valid amount.');
                    return;
                }

                const deposits = JSON.parse(localStorage.getItem('deposits')) || [];
                const newDeposit = {
                    id: deposits.length + 1,
                    method: selectedDepositMethod,
                    amount,
                    currency,
                    details,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                deposits.push(newDeposit);
                saveDeposits(deposits);

                alert(`Deposit request for $${amount.toFixed(2)} via ${selectedDepositMethod} submitted! Status: Pending`);
                resetDepositForm();
                loadTransactionHistory();
            }

            function resetDepositForm() {
                document.getElementById('bank-amount').value = '500';
                document.getElementById('crypto-amount').value = '500';
                document.getElementById('bank-currency').value = 'USD';
                document.getElementById('crypto-currency').value = 'USD';
                if (document.getElementById('bank-proof')) document.getElementById('bank-proof').value = '';
                if (document.getElementById('crypto-proof')) document.getElementById('crypto-proof').value = '';
                updateReferenceCode();
            }

            // Withdrawal Functions
            function updateFeePreview() {
                const amount = parseFloat(document.getElementById('withdraw-amount').value) || 0;
                const fee = amount * feeRate;
                const receiveAmount = amount - fee;
                document.getElementById('fee-preview').textContent = `You will receive: $${receiveAmount.toFixed(2)} (after ${feeRate * 100}% fee)`;
            }

            function submitWithdrawal() {
                console.log(`Submit Withdrawal clicked, method: ${selectedWithdrawMethod}`);
                const amount = parseFloat(document.getElementById('withdraw-amount').value);
                const currency = document.getElementById('withdraw-currency').value;
                const otp = document.getElementById('otp-code').value;

                if (!amount || amount < minWithdrawal) {
                    alert(`Please enter an amount of at least $${minWithdrawal.toFixed(2)}.`);
                    return;
                }
                if (amount > availableBalance) {
                    alert(`Amount exceeds available balance of $${availableBalance.toFixed(2)}.`);
                    return;
                }
                if (!otp) {
                    alert('Please enter a valid OTP/2FA code.');
                    return;
                }

                let details = {};
                if (selectedWithdrawMethod === 'Bank Transfer') {
                    const bankName = document.getElementById('bank-name').value;
                    const accountNumber = document.getElementById('bank-account-number').value;
                    const accountName = document.getElementById('bank-account-name').value;
                    const branch = document.getElementById('bank-branch').value;
                    const saveDefault = document.getElementById('bank-default').checked;
                    if (!bankName || !accountNumber || !accountName) {
                        alert('Please fill in all bank details.');
                        return;
                    }
                    details = {
                        bank_name: bankName,
                        account_number: accountNumber,
                        account_name: accountName,
                        branch: branch || 'N/A',
                        save_default: saveDefault
                    };
                } else if (selectedWithdrawMethod === 'Crypto Wallet') {
                    const coinType = document.getElementById('coin-type').value;
                    const network = document.getElementById('crypto-network').value;
                    const walletAddress = document.getElementById('crypto-wallet').value;
                    if (!coinType || !network || !walletAddress) {
                        alert('Please fill in all crypto details.');
                        return;
                    }
                    details = {
                        coin_type: coinType,
                        network: network,
                        wallet_address: walletAddress
                    };
                }

                const withdrawals = JSON.parse(localStorage.getItem('withdrawals')) || [];
                const newWithdrawal = {
                    id: withdrawals.length + 1,
                    method: selectedWithdrawMethod,
                    amount,
                    currency,
                    details,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                withdrawals.push(newWithdrawal);
                saveWithdrawals(withdrawals);

                alert(`Withdrawal request for $${amount.toFixed(2)} via ${selectedWithdrawMethod} submitted! Status: Pending`);
                resetWithdrawalForm();
                loadTransactionHistory();
            }

            function resetWithdrawalForm() {
                document.getElementById('withdraw-amount').value = '200';
                document.getElementById('withdraw-currency').value = 'USD';
                document.getElementById('bank-name').value = 'GTBank';
                document.getElementById('bank-account-number').value = '';
                document.getElementById('bank-account-name').value = '';
                document.getElementById('bank-branch').value = '';
                document.getElementById('bank-default').checked = false;
                document.getElementById('coin-type').value = 'BTC';
                document.getElementById('crypto-network').value = 'ERC20';
                document.getElementById('crypto-wallet').value = '';
                document.getElementById('otp-code').value = '';
                updateFeePreview();
            }

            // Event Listeners
            if (confirmDeposit) {
                confirmDeposit.addEventListener('click', submitDeposit);
            }
            if (submitWithdrawal) {
                submitWithdrawal.addEventListener('click', submitWithdrawal);
            }
            if (document.getElementById('withdraw-amount')) {
                document.getElementById('withdraw-amount').addEventListener('input', updateFeePreview);
                document.getElementById('withdraw-currency').addEventListener('change', updateFeePreview);
            }

            // Initial Load
            console.log('Initializing dashboard');
            updateReferenceCode();
            updateFeePreview();
            loadTransactionHistory();

            // Prepopulate sample data if empty
            const deposits = JSON.parse(localStorage.getItem('deposits')) || [];
            const withdrawals = JSON.parse(localStorage.getItem('withdrawals')) || [];
            if (deposits.length === 0 && withdrawals.length === 0) {
                console.log('Prepopulating sample transactions');
                saveDeposits([
                    { id: 1, method: 'Bank Transfer', amount: 500, currency: 'USD', details: {}, status: 'Approved', timestamp: '2025-09-01, 12:00:00', user_id: 'user123' },
                    { id: 2, method: 'Bank Transfer', amount: 1000, currency: 'USD', details: {}, status: 'Approved', timestamp: '2025-08-25, 12:00:00', user_id: 'user123' }
                ]);
                saveWithdrawals([
                    { id: 1, method: 'Bank Transfer', amount: 200, currency: 'USD', details: {}, status: 'Pending', timestamp: '2025-08-29, 12:00:00', user_id: 'user123' },
                    { id: 2, method: 'Crypto Wallet', amount: 150, currency: 'USD', details: { coin_type: 'USDT' }, status: 'Rejected', timestamp: '2025-08-20, 12:00:00', user_id: 'user123' }
                ]);
                loadTransactionHistory();
            }
        });
    </script>
</body>
</html>
