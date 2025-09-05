<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Prop Firm Dashboard</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
        header { background: #333; color: white; padding: 10px; display: flex; justify-content: space-between; align-items: center; }
        #hamburger { cursor: pointer; font-size: 24px; z-index: 20; }
        #menu { display: none; position: absolute; top: 50px; left: 10px; background: white; border: 1px solid #ddd; padding: 10px; z-index: 15; }
        #menu ul { list-style: none; padding: 0; margin: 0; }
        #menu li { margin-bottom: 10px; cursor: pointer; }
        #main-content { max-width: 600px; margin: 20px auto; padding: 20px; }
        section { display: none; }
        #dashboard-section { display: block; }
        h2 { font-size: 18px; margin-top: 20px; }
        label { display: block; margin-bottom: 5px; }
        input, select, textarea { width: 100%; padding: 8px; margin-bottom: 10px; box-sizing: border-box; }
        textarea { height: 100px; }
        .account-options, .transaction-list { margin-bottom: 10px; }
        .share-display { font-weight: bold; }
        button { padding: 10px 20px; margin-right: 10px; cursor: pointer; z-index: 10; position: relative; }
        #pending-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 100; }
        #pending-content { background: white; padding: 20px; max-width: 500px; width: 90%; max-height: 80%; overflow-y: auto; }
        #pending-list, #all-requests-list, #recent-deposits, #recent-withdrawals, #trader-requests, #trader-investors, #audit-logs { list-style: none; padding: 0; }
        #pending-list li, #all-requests-list li, #recent-deposits li, #recent-withdrawals li, #trader-requests li, #trader-investors li, #audit-logs li { margin-bottom: 10px; border-bottom: 1px solid #ddd; padding-bottom: 10px; }
        .tab-container { display: flex; margin-bottom: 20px; }
        .tab { padding: 10px 20px; cursor: pointer; border-bottom: 2px solid transparent; }
        .tab.active { border-bottom: 2px solid #333; font-weight: bold; }
        .tab-content { display: none; }
        .tab-content.active { display: block; }
        .method-buttons { display: flex; gap: 10px; margin-bottom: 10px; }
        .method-button { padding: 8px 16px; border: 1px solid #ddd; cursor: pointer; }
        .method-button.active { background: #333; color: white; }
        .confirm-deposit, .submit-managed, .submit-account { background: blue; color: white; }
        .submit-withdraw { background: green; color: white; }
        .decline-button { background: red; color: white; }
        .instructions { background: #f0f0f0; padding: 10px; margin-bottom: 10px; }
        .qr-code { width: 100px; height: 100px; background: #ccc; margin: 10px 0; }
        .copy-button { padding: 5px 10px; font-size: 12px; }
        #bank-name-container, #custom-profit-split { display: none; }
        #bank-name-custom { display: none; }
        #message-modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.5); justify-content: center; align-items: center; z-index: 100; }
        #message-content { background: white; padding: 20px; max-width: 500px; width: 90%; }
        #messages-list { max-height: 300px; overflow-y: auto; }
        #filter-container { margin-bottom: 20px; }
        @media (max-width: 600px) {
            button, .method-button { width: 100%; margin-bottom: 10px; }
            #main-content { padding: 10px; }
        }
    </style>
</head>
<body>
    <header>
        <div id="hamburger">&#9776;</div>
        <h1>Prop Firm Dashboard</h1>
    </header>

  <nav id="menu">
        <ul>
            <li data-section="dashboard-section">Dashboard</li>
            <li data-section="co-funding-section">Co-Funding Request</li>
            <li data-section="wallet-section">Wallet Dashboard</li>
            <li data-section="managed-trading-section">Managed Trading Application</li>
            <li data-section="managed-account-section">Managed Account Request</li>
            <li data-section="trader-dashboard-section">Trader Dashboard</li>
            <li data-section="requests-section">View All Requests</li>
        </ul>
    </nav>

   <div id="main-content">
        <!-- Dashboard Section -->
        <section id="dashboard-section">
            <h2>Welcome to the Dashboard</h2>
            <p>This is the main dashboard. Use the menu to navigate.</p>
            <h2>Recent Results</h2>
            <p>Placeholder for recent results (e.g., success rates, payouts).</p>
        </section>
 <!-- Co-Funding Section -->
        <section id="co-funding-section">
            <h1>Co-Funding Request</h1>
            <h2>Step 1: Select Prop Firm</h2>
            <select id="prop-firm">
                <option value="System/Partner Will Choose">System/Partner Will Choose</option>
                <option value="FTMO">FTMO</option>
                <option value="MyForexFunds">MyForexFunds</option>
                <option value="E8 Funding">E8 Funding</option>
                <option value="The Funded Trader">The Funded Trader</option>
            </select>
            <h2>Step 2: Select Account Size</h2>
            <div id="account-options" class="account-options"></div>
            <h2>Step 3: Challenge Rules</h2>
            <p id="challenge-rules">No specific challenge rules; system or partner will select the Prop Firm and account size.</p>
            <h2>Step 4: Co-Funding Details</h2>
            <label for="profit-split">Profit Split Ratio (e.g., 50:50 for requester:co-funder):</label>
            <input type="text" id="profit-split" value="50:50">
            <label>Your Contribution (Requester's Share):</label>
            <span id="requester-share" class="share-display">N/A</span>
            <label>Co-Funder's Contribution:</label>
            <span id="co-funder-share" class="share-display">N/A</span>
            <button id="submit-request">Submit Request</button>
        </section>
  <!-- Wallet Dashboard Section -->
        <section id="wallet-section">
            <h1>Wallet Dashboard</h1>
            <div class="tab-container">
                <div class="tab active" data-tab="deposit-tab">Deposit</div>
                <div class="tab" data-tab="withdraw-tab">Withdraw</div>
            </div>
            <div id="deposit-tab" class="tab-content active">
                <h2>Deposit Funds</h2>
                <label>Choose Method:</label>
                <div class="method-buttons">
                    <button class="method-button active" data-method="Bank">💳 Bank Transfer</button>
                    <button class="method-button" data-method="Card">💵 Card Payment</button>
                    <button class="method-button" data-method="Crypto">🔗 Crypto Wallet</button>
                </div>
                <label for="deposit-amount">Enter Amount ($):</label>
                <input type="number" id="deposit-amount" min="1" value="500">
                <label for="deposit-currency">Currency:</label>
                <select id="deposit-currency">
                    <option value="USD">USD</option>
                </select>
                <div id="deposit-details"></div>
                <p class="instructions">⚠️ Please make payment to the account/wallet shown above. Your deposit will be confirmed once payment is received.</p>
                <button id="submit-deposit" class="confirm-deposit">✅ Confirm Deposit</button>
                <h2>Recent Deposits</h2>
                <ul id="recent-deposits" class="transaction-list"></ul>
            </div>
            <div id="withdraw-tab" class="tab-content">
                <h2>Withdraw Funds</h2>
                <p><strong>Available Balance:</strong> $2,450.00</p>
                <label for="withdraw-amount">Enter Amount ($):</label>
                <input type="number" id="withdraw-amount" min="1" value="200">
                <label for="withdraw-method">Select Destination Method:</label>
                <div class="method-buttons">
                    <button class="method-button active" data-method="Bank Account">🏦 Bank Account</button>
                    <button class="method-button" data-method="Crypto Wallet">🔗 Crypto Wallet</button>
                    <button class="method-button" data-method="Mobile Money">📲 Mobile Money</button>
                </div>
                <div id="bank-name-container">
                    <label for="bank-name">Bank Name:</label>
                    <select id="bank-name">
                        <option value="GTBank">GTBank</option>
                        <option value="Zenith Bank">Zenith Bank</option>
                        <option value="First Bank">First Bank</option>
                        <option value="Access Bank">Access Bank</option>
                        <option value="Other">Other</option>
                    </select>
                    <input type="text" id="bank-name-custom" placeholder="Enter bank name">
                </div>
                <label for="withdraw-destination">Destination:</label>
                <input type="text" id="withdraw-destination" placeholder="Enter bank account or wallet address">
                <label for="withdraw-security">Security (OTP/PIN):</label>
                <input type="text" id="withdraw-security" placeholder="Enter OTP or PIN">
                <button id="submit-withdraw" class="submit-withdraw">Submit Withdrawal</button>
                <h2>Recent Withdrawals</h2>
                <ul id="recent-withdrawals" class="transaction-list"></ul>
            </div>
        </section>
   <!-- Managed Trading Account Application (Trader-Side) -->
        <section id="managed-trading-section">
            <h1>Managed Trading Account Application</h1>
            <h2>Investment Plan</h2>
            <label for="trader-capital-size">Capital Size ($):</label>
            <input type="number" id="trader-capital-size" min="1000" placeholder="Enter amount (min $1,000)">
            <label for="account-type">Kind of Account:</label>
            <select id="account-type">
                <option value="Forex">Forex</option>
                <option value="Stocks">Stocks</option>
                <option value="Crypto">Crypto</option>
                <option value="Commodities">Commodities</option>
            </select>
            <label for="trader-return-cycle">Return Cycle:</label>
            <select id="trader-return-cycle">
                <option value="Daily">Daily</option>
                <option value="Weekly">Weekly</option>
                <option value="Monthly">Monthly</option>
                <option value="Long-term">Long-term</option>
            </select>
            <label for="trader-profit-target">Profit Target (%):</label>
            <input type="number" id="trader-profit-target" min="0" max="100" placeholder="Enter percentage (0-100)">
            <label for="trader-withdrawal-terms">Withdrawal Terms:</label>
            <input type="text" id="trader-withdrawal-terms" placeholder="Describe withdrawal terms">
            <label for="trader-lot-size">Lot Size:</label>
            <input type="number" id="trader-lot-size" min="0.01" step="0.01" placeholder="Enter lot size (e.g., 0.01)">
            <label for="trader-experience-level">Experience Level:</label>
            <select id="trader-experience-level">
                <option value="Beginner">Beginner</option>
                <option value="Intermediate">Intermediate</option>
                <option value="Expert">Expert</option>
            </select>
            <h2>Proof of Trading Skills</h2>
            <label for="trader-proof-experience">Proof of Experience (Certificates, Prop Firm Passes, Optional):</label>
            <input type="file" id="trader-proof-experience" accept="image/*,application/pdf">
            <label for="trader-proof-trades">Proof of Trades (Screenshots, Optional):</label>
            <input type="file" id="trader-proof-trades" accept="image/*">
            <label for="trader-verification-link">Trading Account Verification Link (Myfxbook, FX Blue, TradingView):</label>
            <input type="text" id="trader-verification-link" placeholder="Enter verification URL">
            <label for="trader-social-media">Social Media Profile (Optional, e.g., Twitter/X, LinkedIn, Telegram):</label>
            <input type="text" id="trader-social-media" placeholder="Enter profile URL">
            <label for="trader-track-record">Track Record Upload (PDF/CSV, Optional):</label>
            <input type="file" id="trader-track-record" accept=".pdf,.csv">
            <label for="trader-trading-style">Trading Style / Short Bio:</label>
            <textarea id="trader-trading-style" placeholder="Describe your strategy, risk management, and approach"></textarea>
            <h2>Optional Extras</h2>
            <label for="trader-risk-tolerance">Risk Tolerance:</label>
            <select id="trader-risk-tolerance">
                <option value="">Select (Optional)</option>
                <option value="Conservative">Conservative</option>
                <option value="Moderate">Moderate</option>
                <option value="Aggressive">Aggressive</option>
            </select>
            <label for="trader-referral">Referral / Recommendation (Optional):</label>
            <input type="text" id="trader-referral" placeholder="Enter referral details">
            <label>
                <input type="checkbox" id="trader-terms-agreement">
                I agree to the terms and conditions
            </label>
            <button id="submit-managed" class="submit-managed">Submit Application</button>
        </section>
<!-- Managed Account Request (Investor-Side) -->
        <section id="managed-account-section">
            <h1>Managed Account Request</h1>
            <h2>Account Details</h2>
            <label for="broker-name">Broker Name:</label>
            <input type="text" id="broker-name" placeholder="Enter broker name">
            <label for="account-id">Account ID:</label>
            <input type="text" id="account-id" placeholder="Enter account ID">
            <label for="trader-platform-id">Trader’s Platform ID / Username:</label>
            <input type="text" id="trader-platform-id" placeholder="Enter trader’s platform ID/username">
            <h2>Investment Plan</h2>
            <label for="investor-capital-size">Capital Size ($):</label>
            <input type="number" id="investor-capital-size" min="1000" placeholder="Enter amount (min $1,000)">
            <label for="profit-split">Profit Split (%):</label>
            <select id="profit-split">
                <option value="50/50">50/50</option>
                <option value="60/40">60/40 (Investor/Trader)</option>
                <option value="70/30">70/30 (Investor/Trader)</option>
                <option value="Custom">Custom</option>
            </select>
            <input type="text" id="custom-profit-split" placeholder="Enter custom split (e.g., 55/45)">
            <label for="investor-return-cycle">Target Amount / Return Cycle:</label>
            <select id="investor-return-cycle">
                <option value="Daily">Daily</option>
                <option value="Weekly">Weekly</option>
                <option value="Monthly">Monthly</option>
                <option value="Long-term">Long-term</option>
            </select>
            <label for="investor-profit-target">Profit Target (%):</label>
            <input type="number" id="investor-profit-target" min="0" max="100" placeholder="Enter percentage (0-100)">
            <h2>Additional Security & Flexibility</h2>
            <label for="investor-password">Investor Password / API Key (Optional, read-only):</label>
            <input type="text" id="investor-password" placeholder="Enter password or API key (encrypted)">
            <label for="investor-risk-tolerance">Risk Tolerance:</label>
            <select id="investor-risk-tolerance">
                <option value="">Select (Optional)</option>
                <option value="Conservative">Conservative</option>
                <option value="Moderate">Moderate</option>
                <option value="Aggressive">Aggressive</option>
            </select>
            <label for="start-date">Start Date (Optional):</label>
            <input type="date" id="start-date">
            <label for="duration">Duration (Optional):</label>
            <select id="duration">
                <option value="">Select (Optional)</option>
                <option value="1 Month">1 Month</option>
                <option value="3 Months">3 Months</option>
                <option value="6 Months">6 Months</option>
                <option value="1 Year">1 Year</option>
            </select>
            <label for="account-proof">Document Upload (Optional, Proof of Account Ownership):</label>
            <input type="file" id="account-proof" accept="image/*,application/pdf">
            <label>
                <input type="checkbox" id="investor-terms-agreement">
                I authorize the assigned trader to manage this account under the agreed profit-sharing terms
            </label>
            <button id="submit-account" class="submit-account">Submit Request</button>
        </section>
  <!-- Trader Dashboard Section -->
        <section id="trader-dashboard-section">
            <h1>Trader Dashboard</h1>
            <h2>Your Managed Trading Applications</h2>
            <ul id="trader-requests" class="transaction-list"></ul>
            <h2>Linked Investor Requests</h2>
            <ul id="trader-investors" class="transaction-list"></ul>
            <h2>Messages</h2>
            <button id="open-messages">View Messages</button>
        </section>
 <!-- View All Requests Section (Investor/Admin Dashboard) -->
        <section id="requests-section">
            <h1>Request & Agreement Management</h1>
            <div id="filter-container">
                <label for="filter-status">Filter by Status:</label>
                <select id="filter-status">
                    <option value="All">All</option>
                    <option value="Pending">Pending</option>
                    <option value="Active">Active</option>
                    <option value="Completed">Completed</option>
                    <option value="On Hold">On Hold</option>
                    <option value="Suspended">Suspended</option>
                </select>
                <label for="filter-id">Filter by Trader/Investor ID:</label>
                <input type="text" id="filter-id" placeholder="Enter ID">
                <button id="apply-filter">Apply Filter</button>
            </div>
            <h2>All Requests & Agreements</h2>
            <ul id="all-requests-list"></ul>
            <h2>Audit Logs (Admin Only)</h2>
            <ul id="audit-logs"></ul>
            <button id="view-pending">View and Manage Pending Requests</button>
            <button id="export-history">Export Agreement History</button>
        </section>
    </div>
  <!-- Pending Requests Modal -->
    <div id="pending-modal">
        <div id="pending-content">
            <h2>Pending Requests</h2>
            <ul id="pending-list"></ul>
            <button id="close-modal">Close</button>
        </div>
    </div>
  <!-- Messaging Modal -->
    <div id="message-modal">
        <div id="message-content">
            <h2>Messages</h2>
            <label for="message-recipient">Recipient (Trader/Investor ID):</label>
            <input type="text" id="message-recipient" placeholder="Enter ID">
            <label for="message-text">Message:</label>
            <textarea id="message-text" placeholder="Type your message"></textarea>
            <button id="send-message">Send</button>
            <h3>Conversation</h3>
            <div id="messages-list"></div>
            <button id="close-message-modal">Close</button>
        </div>
    </div>

   <script>
        document.addEventListener('DOMContentLoaded', () => {
            console.log('DOM loaded, initializing dashboard...');

            // Prop Firm Data
            const propFirms = {
                "FTMO": {
                    "accounts": [
                        { "size": "$1,000", "price": 13.0 },
                        { "size": "$2,000", "price": 17.0 },
                        { "size": "$5,000", "price": 30.0 },
                        { "size": "$10,000", "price": 155.0 },
                        { "size": "$100,000", "price": 500.0 },
                        { "size": "$200,000", "price": 1080.0 }
                    ],
                    "challenge_rules": "Must hit 10% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily loss, 10% max drawdown."
                },
                "MyForexFunds": {
                    "accounts": [
                        { "size": "$10,000", "price": 49.0 },
                        { "size": "$50,000", "price": 299.0 },
                        { "size": "$200,000", "price": 999.0 }
                    ],
                    "challenge_rules": "Must hit 8% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily drawdown, 12% max drawdown."
                },
                "E8 Funding": {
                    "accounts": [
                        { "size": "$25,000", "price": 228.0 },
                        { "size": "$100,000", "price": 398.0 },
                        { "size": "$400,000", "price": 998.0 }
                    ],
                    "challenge_rules": "Must hit 8% profit target in 30 days (Step 1), 5% in 60 days (Step 2), max 5% daily drawdown, 8% max loss."
                },
                "The Funded Trader": {
                    "accounts": [
                        { "size": "$50,000", "price": 299.0 },
                        { "size": "$100,000", "price": 499.0 },
                        { "size": "$200,000", "price": 899.0 }
                    ],
                    "challenge_rules": "Must hit 10% profit target in 35 days (Step 1), 5% in 60 days (Step 2), max 6% daily loss, 12% max drawdown."
                },
                "System/Partner Will Choose": {
                    "accounts": [{ "size": "N/A", "price": 0.0 }],
                    "challenge_rules": "No specific challenge rules; system or partner will select the Prop Firm and account size."
                }
            };

            // DOM Elements
            const hamburger = document.getElementById('hamburger');
            const menu = document.getElementById('menu');
            const menuItems = menu.querySelectorAll('li');
            const sections = document.querySelectorAll('section');
            const firmSelect = document.getElementById('prop-firm');
            const accountOptions = document.getElementById('account-options');
            const challengeRules = document.getElementById('challenge-rules');
            const profitSplitInput = document.getElementById('profit-split');
            const requesterShare = document.getElementById('requester-share');
            const coFunderShare = document.getElementById('co-funder-share');
            const submitButton = document.getElementById('submit-request');
            const viewPendingButton = document.getElementById('view-pending');
            const pendingModal = document.getElementById('pending-modal');
            const pendingList = document.getElementById('pending-list');
            const closeModalButton = document.getElementById('close-modal');
            const allRequestsList = document.getElementById('all-requests-list');
            const exportHistoryButton = document.getElementById('export-history');
            const tabs = document.querySelectorAll('.tab');
            const tabContents = document.querySelectorAll('.tab-content');
            const depositAmount = document.getElementById('deposit-amount');
            const depositCurrency = document.getElementById('deposit-currency');
            const depositDetails = document.getElementById('deposit-details');
            const submitDeposit = document.getElementById('submit-deposit');
            const withdrawAmount = document.getElementById('withdraw-amount');
            const withdrawMethodButtons = document.querySelectorAll('#withdraw-tab .method-button');
            const withdrawDestination = document.getElementById('withdraw-destination');
            const withdrawSecurity = document.getElementById('withdraw-security');
            const bankNameSelect = document.getElementById('bank-name');
            const bankNameCustom = document.getElementById('bank-name-custom');
            const bankNameContainer = document.getElementById('bank-name-container');
            const submitWithdraw = document.getElementById('submit-withdraw');
            const recentDeposits = document.getElementById('recent-deposits');
            const recentWithdrawals = document.getElementById('recent-withdrawals');
            const depositMethodButtons = document.querySelectorAll('#deposit-tab .method-button');
            const traderCapitalSize = document.getElementById('trader-capital-size');
            const accountType = document.getElementById('account-type');
            const traderReturnCycle = document.getElementById('trader-return-cycle');
            const traderProfitTarget = document.getElementById('trader-profit-target');
            const traderWithdrawalTerms = document.getElementById('trader-withdrawal-terms');
            const traderLotSize = document.getElementById('trader-lot-size');
            const traderExperienceLevel = document.getElementById('trader-experience-level');
            const traderProofExperience = document.getElementById('trader-proof-experience');
            const traderProofTrades = document.getElementById('trader-proof-trades');
            const traderVerificationLink = document.getElementById('trader-verification-link');
            const traderSocialMedia = document.getElementById('trader-social-media');
            const traderTrackRecord = document.getElementById('trader-track-record');
            const traderTradingStyle = document.getElementById('trader-trading-style');
            const traderRiskTolerance = document.getElementById('trader-risk-tolerance');
            const traderReferral = document.getElementById('trader-referral');
            const traderTermsAgreement = document.getElementById('trader-terms-agreement');
            const submitManaged = document.getElementById('submit-managed');
            const brokerName = document.getElementById('broker-name');
            const accountId = document.getElementById('account-id');
            const traderPlatformId = document.getElementById('trader-platform-id');
            const profitSplit = document.getElementById('profit-split');
            const customProfitSplit = document.getElementById('custom-profit-split');
            const investorReturnCycle = document.getElementById('investor-return-cycle');
            const investorProfitTarget = document.getElementById('investor-profit-target');
            const investorPassword = document.getElementById('investor-password');
            const investorRiskTolerance = document.getElementById('investor-risk-tolerance');
            const startDate = document.getElementById('start-date');
            const duration = document.getElementById('duration');
            const accountProof = document.getElementById('account-proof');
            const investorTermsAgreement = document.getElementById('investor-terms-agreement');
            const submitAccount = document.getElementById('submit-account');
            const traderRequests = document.getElementById('trader-requests');
            const traderInvestors = document.getElementById('trader-investors');
            const openMessages = document.getElementById('open-messages');
            const filterStatus = document.getElementById('filter-status');
            const filterId = document.getElementById('filter-id');
            const applyFilter = document.getElementById('apply-filter');
            const auditLogs = document.getElementById('audit-logs');
            const messageModal = document.getElementById('message-modal');
            const messageRecipient = document.getElementById('message-recipient');
            const messageText = document.getElementById('message-text');
            const sendMessage = document.getElementById('send-message');
            const messagesList = document.getElementById('messages-list');
            const closeMessageModal = document.getElementById('close-message-modal');

            let selectedAccount = 'N/A';
            let accountPrice = 0.0;
            let selectedDepositMethod = 'Bank';
            let selectedWithdrawMethod = 'Bank Account';
            const userId = 'user123'; // Simulated user ID

            // Debug all button clicks
            document.querySelectorAll('button').forEach(btn => {
                btn.addEventListener('click', () => {
                    console.log(`Button clicked: ${btn.id || btn.textContent}`);
                });
            });

            // LocalStorage Functions
            function loadRequests() {
                try {
                    return JSON.parse(localStorage.getItem('requests')) || [];
                } catch (e) {
                    console.error('Error loading requests:', e);
                    return [];
                }
            }

            function loadMessages() {
                try {
                    return JSON.parse(localStorage.getItem('messages')) || [];
                } catch (e) {
                    console.error('Error loading messages:', e);
                    return [];
                }
            }

            function loadAuditLogs() {
                try {
                    return JSON.parse(localStorage.getItem('audit_logs')) || [];
                } catch (e) {
                    console.error('Error loading audit logs:', e);
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

            function saveMessages(messages) {
                try {
                    localStorage.setItem('messages', JSON.stringify(messages));
                } catch (e) {
                    console.error('Error saving messages:', e);
                    alert('Error saving message. Check console.');
                }
            }

            function saveAuditLog(action, details) {
                try {
                    const logs = loadAuditLogs();
                    logs.push({
                        action,
                        details,
                        timestamp: new Date().toLocaleString('en-US')
                    });
                    localStorage.setItem('audit_logs', JSON.stringify(logs));
                } catch (e) {
                    console.error('Error saving audit log:', e);
                }
            }

            function generateReferenceCode() {
                return 'REF-' + Math.random().toString(36).substr(2, 4).toUpperCase();
            }

            // Menu and Section Handling
            if (hamburger && menu) {
                hamburger.addEventListener('click', () => {
                    console.log('Hamburger menu clicked');
                    menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                });
            }

            menuItems.forEach(item => {
                item.addEventListener('click', () => {
                    console.log(`Menu item clicked: ${item.dataset.section}`);
                    sections.forEach(sec => sec.style.display = 'none');
                    const section = document.getElementById(item.dataset.section);
                    if (section) {
                        section.style.display = 'block';
                    } else {
                        console.error(`Section ${item.dataset.section} not found`);
                    }
                    menu.style.display = 'none';
                    if (item.dataset.section === 'requests-section') {
                        loadAllRequests();
                        loadAuditLogs();
                    } else if (item.dataset.section === 'wallet-section') {
                        updateDepositDetails();
                        updateWithdrawDetails();
                        loadRecentTransactions();
                    } else if (item.dataset.section === 'trader-dashboard-section') {
                        loadTraderDashboard();
                    }
                });
            });

            // Co-Funding Functions
            function updateAccountSizes() {
                console.log('Updating account sizes for firm:', firmSelect.value);
                const firm = firmSelect.value;
                accountOptions.innerHTML = '';
                selectedAccount = 'N/A';
                accountPrice = 0.0;

                propFirms[firm].accounts.forEach(acc => {
                    const label = document.createElement('label');
                    const radio = document.createElement('input');
                    radio.type = 'radio';
                    radio.name = 'account-size';
                    radio.value = acc.size;
                    radio.addEventListener('change', () => {
                        console.log('Account size selected:', acc.size);
                        selectedAccount = acc.size;
                        accountPrice = acc.price;
                        calculateShares();
                    });
                    label.appendChild(radio);
                    label.appendChild(document.createTextNode(` ${acc.size} - $${acc.price.toFixed(2)}`));
                    accountOptions.appendChild(label);
                });

                challengeRules.textContent = propFirms[firm].challenge_rules;
                calculateShares();
            }

            function calculateShares() {
                console.log('Calculating shares');
                const profitSplit = profitSplitInput.value || '50:50';
                if (selectedAccount === 'N/A') {
                    requesterShare.textContent = 'N/A';
                    coFunderShare.textContent = 'N/A';
                    return;
                }

                try {
                    const [reqPart, coPart] = profitSplit.split(':').map(Number);
                    const totalParts = reqPart + coPart;
                    if (totalParts === 0) throw new Error('Invalid profit split');
                    const reqShare = (reqPart / totalParts) * accountPrice;
                    const coShare = (coPart / totalParts) * accountPrice;
                    requesterShare.textContent = `$${reqShare.toFixed(2)} (Locked in Escrow)`;
                    coFunderShare.textContent = `$${coShare.toFixed(2)} (Awaiting Co-Funder)`;
                } catch {
                    requesterShare.textContent = 'Invalid Ratio';
                    coFunderShare.textContent = 'Invalid Ratio';
                }
            }

            function submitRequest() {
                console.log('Submit Co-Funding Request clicked');
                const firm = firmSelect.value;
                if (firm === 'System/Partner Will Choose' || selectedAccount === 'N/A') {
                    alert('Please select a valid Prop Firm and Account Size.');
                    return;
                }

                const profitSplit = profitSplitInput.value || '50:50';
                if (!profitSplit.includes(':')) {
                    alert('Please enter a valid profit split ratio (e.g., 50:50).');
                    return;
                }

                const reqShareText = requesterShare.textContent;
                if (reqShareText.includes('Invalid')) {
                    alert('Invalid share calculation.');
                    return;
                }

                const reqShareVal = parseFloat(reqShareText.replace('$', '').split(' ')[0]);
                const coShareVal = parseFloat(coFunderShare.textContent.replace('$', '').split(' ')[0]);

                const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'co-funding',
                    prop_firm: firm,
                    account_size: selectedAccount,
                    account_price: accountPrice,
                    profit_split: profitSplit,
                    requester_share: reqShareVal,
                    co_funder_share: coShareVal,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);
                saveAuditLog('submit_co_funding', `User ${userId} submitted co-funding request ID ${newRequest.id}`);

                alert(`Co-Funding Request submitted! Your contribution: $${reqShareVal.toFixed(2)} (Locked in Escrow). Waiting for a co-funder... Status: Pending`);
                profitSplitInput.value = '50:50';
                firmSelect.value = 'System/Partner Will Choose';
                updateAccountSizes();
            }

            // Wallet Functions
            function updateDepositDetails() {
                console.log('Updating deposit details for method:', selectedDepositMethod);
                const details = {
                    'Bank': `
                        <p><strong>Payment Details:</strong></p>
                        <p>Bank Name: GTBank</p>
                        <p>Account Name: Prop Firm Ltd</p>
                        <p>Account Number: 0123456789</p>
                        <p>Reference Code: ${generateReferenceCode()}</p>
                        <label for="payment-proof">Upload Proof of Payment (Optional):</label>
                        <input type="file" id="payment-proof" accept="image/*,application/pdf">
                    `,
                    'Card': `
                        <p><strong>Payment Details:</strong></p>
                        <label for="card-number">Card Number:</label>
                        <input type="text" id="card-number" placeholder="1234-5678-9012-3456">
                        <label for="card-expiry">Expiry (MM/YY):</label>
                        <input type="text" id="card-expiry" placeholder="MM/YY">
                        <label for="card-cvv">CVV:</label>
                        <input type="text" id="card-cvv" placeholder="123">
                    `,
                    'Crypto': `
                        <p><strong>Payment Details:</strong></p>
                        <label for="coin-type">Select Coin:</label>
                        <select id="coin-type">
                            <option value="BTC">BTC</option>
                            <option value="USDT">USDT</option>
                            <option value="ETH">ETH</option>
                        </select>
                        <p>Wallet Address: 0xAB123456789CDEFFED1234...</p>
                        <button class="copy-button" onclick="navigator.clipboard.writeText('0xAB123456789CDEFFED1234...').then(() => alert('Address copied!'))">Copy Address</button>
                        <p>QR Code:</p>
                        <div class="qr-code"></div>
                        <label for="payment-proof">Upload Proof of Payment (Optional):</label>
                        <input type="file" id="payment-proof" accept="image/*,application/pdf">
                    `
                };
                depositDetails.innerHTML = details[selectedDepositMethod];
            }

            function updateWithdrawDetails() {
                console.log('Updating withdraw details for method:', selectedWithdrawMethod);
                const placeholder = selectedWithdrawMethod === 'Crypto Wallet' ? 'Enter wallet address' : selectedWithdrawMethod === 'Mobile Money' ? 'Enter mobile number' : 'Enter bank account number';
                withdrawDestination.placeholder = placeholder;
                bankNameContainer.style.display = selectedWithdrawMethod === 'Bank Account' ? 'block' : 'none';
                bankNameCustom.style.display = bankNameSelect.value === 'Other' ? 'block' : 'none';
            }

            function submitDepositRequest() {
                console.log('Submit Deposit clicked');
                const amount = parseFloat(depositAmount.value);
                if (!amount || amount <= 0) {
                    alert('Please enter a valid deposit amount.');
                    return;
                }

                let details = {};
                if (selectedDepositMethod === 'Bank' || selectedDepositMethod === 'Crypto') {
                    const proof = document.getElementById('payment-proof')?.files[0];
                    details.proof = proof ? proof.name : 'No proof uploaded';
                    details.reference_code = generateReferenceCode();
                } else if (selectedDepositMethod === 'Card') {
                    const cardNumber = document.getElementById('card-number')?.value;
                    const cardExpiry = document.getElementById('card-expiry')?.value;
                    const cardCvv = document.getElementById('card-cvv')?.value;
                    if (!cardNumber || !cardExpiry || !cardCvv) {
                        alert('Please enter valid card details.');
                        return;
                    }
                    details.card_number = cardNumber;
                    details.card_expiry = cardExpiry;
                    details.card_cvv = cardCvv;
                }
                if (selectedDepositMethod === 'Crypto') {
                    details.coin_type = document.getElementById('coin-type')?.value || 'BTC';
                    details.wallet_address = '0xAB123456789CDEFFED1234...';
                }

                const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'deposit',
                    amount: amount,
                    method: selectedDepositMethod,
                    currency: depositCurrency.value,
                    details: details,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);
                saveAuditLog('submit_deposit', `User ${userId} submitted deposit request ID ${newRequest.id}`);

                alert(`Deposit request for $${amount.toFixed(2)} via ${selectedDepositMethod} submitted! Status: Pending`);
                depositAmount.value = '500';
                if (document.getElementById('payment-proof')) document.getElementById('payment-proof').value = '';
                if (document.getElementById('card-number')) document.getElementById('card-number').value = '';
                if (document.getElementById('card-expiry')) document.getElementById('card-expiry').value = '';
                if (document.getElementById('card-cvv')) document.getElementById('card-cvv').value = '';
                loadRecentTransactions();
            }

            function submitWithdrawRequest() {
                console.log('Submit Withdraw clicked');
                const amount = parseFloat(withdrawAmount.value);
                if (!amount || amount <= 0) {
                    alert('Please enter a valid withdrawal amount.');
                    return;
                }
                if (amount > 2450) {
                    alert('Withdrawal amount exceeds available balance ($2,450.00).');
                    return;
                }

                const destination = withdrawDestination.value;
                if (!destination) {
                    alert('Please enter a valid destination.');
                    return;
                }

                const security = withdrawSecurity.value;
                if (!security) {
                    alert('Please enter OTP or PIN.');
                    return;
                }

                let bankName = null;
                if (selectedWithdrawMethod === 'Bank Account') {
                    bankName = bankNameSelect.value === 'Other' ? bankNameCustom.value : bankNameSelect.value;
                    if (!bankName) {
                        alert('Please select or enter a valid bank name.');
                        return;
                    }
                }

                const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'withdraw',
                    amount: amount,
                    method: selectedWithdrawMethod,
                    destination: destination,
                    bank_name: bankName,
                    security: security,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);
                saveAuditLog('submit_withdraw', `User ${userId} submitted withdrawal request ID ${newRequest.id}`);

                alert(`Withdrawal request for $${amount.toFixed(2)} via ${selectedWithdrawMethod} submitted! Status: Pending`);
                withdrawAmount.value = '200';
                withdrawDestination.value = '';
                withdrawSecurity.value = '';
                bankNameSelect.value = 'GTBank';
                bankNameCustom.value = '';
                bankNameCustom.style.display = 'none';
                loadRecentTransactions();
            }

            // Managed Trading Functions
            function submitManagedRequest() {
                console.log('Submit Managed Trading clicked');
                const capital = parseFloat(traderCapitalSize.value);
                const profit = parseFloat(traderProfitTarget.value);
                const lot = parseFloat(traderLotSize.value);
                const proofFile = traderProofExperience.files[0];
                const tradesFile = traderProofTrades.files[0];
                const trackFile = traderTrackRecord.files[0];

                if (!capital || capital < 1000) {
                    alert('Please enter a valid capital size (minimum $1,000).');
                    return;
                }
                if (!profit || profit < 0 || profit > 100) {
                    alert('Please enter a valid profit target (0-100%).');
                    return;
                }
                if (!lot || lot < 0.01) {
                    alert('Please enter a valid lot size (minimum 0.01).');
                    return;
                }
                if (!traderWithdrawalTerms.value) {
                    alert('Please describe withdrawal terms.');
                    return;
                }
                if (!traderVerificationLink.value) {
                    alert('Please provide a trading account verification link.');
                    return;
                }
                if (!traderTradingStyle.value) {
                    alert('Please describe your trading style and bio.');
                    return;
                }
                if (!traderTermsAgreement.checked) {
                    alert('You must agree to the terms and conditions.');
                    return;
                }

                const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'managed-trading',
                    capital_size: capital,
                    account_type: accountType.value,
                    return_cycle: traderReturnCycle.value,
                    profit_target: profit,
                    withdrawal_terms: traderWithdrawalTerms.value,
                    lot_size: lot,
                    experience_level: traderExperienceLevel.value,
                    proof_experience: proofFile ? proofFile.name : 'No proof uploaded',
                    proof_trades: tradesFile ? tradesFile.name : 'No trades uploaded',
                    verification_link: traderVerificationLink.value,
                    social_media: traderSocialMedia.value || null,
                    track_record: trackFile ? trackFile.name : 'No track record uploaded',
                    trading_style: traderTradingStyle.value,
                    risk_tolerance: traderRiskTolerance.value || null,
                    referral: traderReferral.value || null,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);
                saveAuditLog('submit_managed_trading', `User ${userId} submitted managed trading application ID ${newRequest.id}`);

                alert(`Managed Trading Account Application submitted! Status: Pending`);
                traderCapitalSize.value = '';
                accountType.value = 'Forex';
                traderReturnCycle.value = 'Daily';
                traderProfitTarget.value = '';
                traderWithdrawalTerms.value = '';
                traderLotSize.value = '';
                traderExperienceLevel.value = 'Beginner';
                traderProofExperience.value = '';
                traderProofTrades.value = '';
                traderVerificationLink.value = '';
                traderSocialMedia.value = '';
                traderTrackRecord.value = '';
                traderTradingStyle.value = '';
                traderRiskTolerance.value = '';
                traderReferral.value = '';
                traderTermsAgreement.checked = false;
                loadAllRequests();
                loadTraderDashboard();
            }

            // Managed Account Functions
            function submitAccountRequest() {
                console.log('Submit Managed Account clicked');
                const capital = parseFloat(investorCapitalSize.value);
                const profit = parseFloat(investorProfitTarget.value);
                const split = profitSplit.value === 'Custom' ? customProfitSplit.value : profitSplit.value;

                if (!brokerName.value) {
                    alert('Please enter a valid broker name.');
                    return;
                }
                if (!accountId.value) {
                    alert('Please enter a valid account ID.');
                    return;
                }
                if (!traderPlatformId.value) {
                    alert('Please enter the trader’s platform ID/username.');
                    return;
                }
                if (!capital || capital < 1000) {
                    alert('Please enter a valid capital size (minimum $1,000).');
                    return;
                }
                if (!split || (profitSplit.value === 'Custom' && !split.match(/^\d+\/\d+$/))) {
                    alert('Please select or enter a valid profit split (e.g., 50/50 or 55/45).');
                    return;
                }
                if (!profit || profit < 0 || profit > 100) {
                    alert('Please enter a valid profit target (0-100%).');
                    return;
                }
                if (!investorTermsAgreement.checked) {
                    alert('You must authorize the trader to manage the account.');
                    return;
                }

                const requests = loadRequests();
                const newRequest = {
                    id: requests.length + 1,
                    type: 'managed-account',
                    broker_name: brokerName.value,
                    account_id: accountId.value,
                    trader_platform_id: traderPlatformId.value,
                    capital_size: capital,
                    profit_split: split,
                    return_cycle: investorReturnCycle.value,
                    profit_target: profit,
                    investor_password: investorPassword.value ? `ENCRYPTED_${investorPassword.value}` : null,
                    risk_tolerance: investorRiskTolerance.value || null,
                    start_date: startDate.value || null,
                    duration: duration.value || null,
                    account_proof: accountProof.files[0]?.name || null,
                    status: 'Pending',
                    timestamp: new Date().toLocaleString('en-US'),
                    user_id: userId
                };
                requests.push(newRequest);
                saveRequests(requests);
                saveAuditLog('submit_managed_account', `User ${userId} submitted managed account request ID ${newRequest.id}`);

                alert(`Managed Account Request submitted! Status: Pending`);
                brokerName.value = '';
                accountId.value = '';
                traderPlatformId.value = '';
                investorCapitalSize.value = '';
                profitSplit.value = '50/50';
                customProfitSplit.value = '';
                customProfitSplit.style.display = 'none';
                investorReturnCycle.value = 'Daily';
                investorProfitTarget.value = '';
                investorPassword.value = '';
                investorRiskTolerance.value = '';
                startDate.value = '';
                duration.value = '';
                accountProof.value = '';
                investorTermsAgreement.checked = false;
                loadAllRequests();
                loadTraderDashboard();
            }

            // Dashboard Functions
            function loadRecentTransactions() {
                console.log('Loading recent transactions');
                const requests = loadRequests();
                recentDeposits.innerHTML = '';
                recentWithdrawals.innerHTML = '';

                const deposits = requests.filter(req => req.type === 'deposit' && req.user_id === userId).slice(-5);
                deposits.forEach(req => {
                    const li = document.createElement('li');
                    const method = req.method === 'Crypto' ? `Crypto (${req.details.coin_type || 'Unknown'})` : req.method;
                    li.textContent = `${req.timestamp} | ${method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ ' + req.status}`;
                    recentDeposits.appendChild(li);
                });
                if (deposits.length === 0) recentDeposits.innerHTML = '<li>No recent deposits.</li>';

                const withdrawals = requests.filter(req => req.type === 'withdraw' && req.user_id === userId).slice(-5);
                withdrawals.forEach(req => {
                    const li = document.createElement('li');
                    const method = req.bank_name ? `${req.method} (${req.bank_name})` : req.method;
                    li.textContent = `${req.timestamp} | ${method} | $${req.amount.toFixed(2)} | ${req.status === 'Completed' ? '✅ Completed' : '⏳ ' + req.status}`;
                    recentWithdrawals.appendChild(li);
                });
                if (withdrawals.length === 0) recentWithdrawals.innerHTML = '<li>No recent withdrawals.</li>';
            }

            function loadTraderDashboard() {
                console.log('Loading trader dashboard');
                const requests = loadRequests();
                traderRequests.innerHTML = '';
                traderInvestors.innerHTML = '';

                const traderApps = requests.filter(req => req.type === 'managed-trading' && req.user_id === userId);
                traderApps.forEach(req => {
                    const li = document.createElement('li');
                    let text = `ID: ${req.id} | Account: ${req.account_type} | Capital: $${req.capital_size.toFixed(2)} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | Status: ${req.status} | Timestamp: ${req.timestamp}`;
                    li.innerHTML = text;
                    traderRequests.appendChild(li);
                });
                if (traderApps.length === 0) traderRequests.innerHTML = '<li>No managed trading applications.</li>';

                const investorRequests = requests.filter(req => req.type === 'managed-account' && req.trader_platform_id === userId);
                investorRequests.forEach(req => {
                    const li = document.createElement('li');
                    let text = `ID: ${req.id} | Broker: ${req.broker_name} | Account ID: ${req.account_id} | Capital: $${req.capital_size.toFixed(2)} | Profit Split: ${req.profit_split} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | Status: ${req.status} | Timestamp: ${req.timestamp}`;
                    li.innerHTML = text;
                    traderInvestors.appendChild(li);
                });
                if (investorRequests.length === 0) traderInvestors.innerHTML = '<li>No linked investor requests.</li>';
            }

            function loadAuditLogs() {
                console.log('Loading audit logs');
                const logs = loadAuditLogs();
                auditLogs.innerHTML = '';
                logs.forEach(log => {
                    const li = document.createElement('li');
                    li.textContent = `${log.timestamp} | ${log.action} | ${log.details}`;
                    auditLogs.appendChild(li);
                });
                if (logs.length === 0) auditLogs.innerHTML = '<li>No audit logs.</li>';
            }

            function viewPending() {
                console.log('View Pending Requests clicked');
                const requests = loadRequests();
                const pending = requests.filter(req => (req.type === 'co-funding' || req.type === 'managed-trading' || req.type === 'managed-account') && req.status === 'Pending');
                pendingList.innerHTML = '';

                if (pending.length === 0) {
                    alert('No pending requests.');
                    return;
                }

                pending.forEach(req => {
                    const li = document.createElement('li');
                    let text = `ID: ${req.id} | Type: ${req.type} | `;
                    if (req.type === 'co-funding') {
                        text += `Firm: ${req.prop_firm} | Size: ${req.account_size} | Price: $${req.account_price.toFixed(2)} | Profit Split: ${req.profit_split} | User: ${req.user_id} | Timestamp: ${req.timestamp}`;
                        const acceptButton = document.createElement('button');
                        acceptButton.textContent = 'Accept as Co-Funder';
                        acceptButton.onclick = () => acceptRequest(req.id);
                        const declineButton = document.createElement('button');
                        declineButton.textContent = 'Decline';
                        declineButton.className = 'decline-button';
                        declineButton.onclick = () => declineRequest(req.id);
                        li.appendChild(acceptButton);
                        li.appendChild(declineButton);
                    } else if (req.type === 'managed-trading') {
                        text += `Account: ${req.account_type} | Capital: $${req.capital_size.toFixed(2)} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | User: ${req.user_id} | Timestamp: ${req.timestamp}`;
                        const acceptButton = document.createElement('button');
                        acceptButton.textContent = 'Accept Application';
                        acceptButton.onclick = () => acceptManagedRequest(req.id, 'managed-trading');
                        const declineButton = document.createElement('button');
                        declineButton.textContent = 'Decline';
                        declineButton.className = 'decline-button';
                        declineButton.onclick = () => declineRequest(req.id);
                        li.appendChild(acceptButton);
                        li.appendChild(declineButton);
                    } else if (req.type === 'managed-account') {
                        text += `Broker: ${req.broker_name} | Account ID: ${req.account_id} | Trader ID: ${req.trader_platform_id} | Capital: $${req.capital_size.toFixed(2)} | Profit Split: ${req.profit_split} | User: ${req.user_id} | Timestamp: ${req.timestamp}`;
                        const acceptButton = document.createElement('button');
                        acceptButton.textContent = 'Approve Request';
                        acceptButton.onclick = () => acceptManagedRequest(req.id, 'managed-account');
                        const declineButton = document.createElement('button');
                        declineButton.textContent = 'Decline';
                        declineButton.className = 'decline-button';
                        declineButton.onclick = () => declineRequest(req.id);
                        li.appendChild(acceptButton);
                        li.appendChild(declineButton);
                    }
                    li.textContent = text;
                    pendingList.appendChild(li);
                });

                pendingModal.style.display = 'flex';
            }

            function acceptRequest(id) {
                console.log(`Accepting co-funding request ID ${id}`);
                const requests = loadRequests();
                const req = requests.find(r => r.id === id && r.type === 'co-funding' && r.status === 'Pending');
                if (!req) return;

                const total = req.requester_share + req.co_funder_share;
                if (Math.abs(total - req.account_price) > 0.01) {
                    alert('Total shares do not match account price.');
                    return;
                }

                req.status = 'Active';
                saveRequests(requests);
                saveAuditLog('accept_co_funding', `Admin accepted co-funding request ID ${id}`);
                alert(`Co-Funding Request ID ${id} accepted! Total funded: $${req.account_price.toFixed(2)}. Status: Active.`);
                pendingModal.style.display = 'none';
                loadAllRequests();
            }

            function acceptManagedRequest(id, type) {
                console.log(`Accepting ${type} request ID ${id}`);
                const requests = loadRequests();
                const req = requests.find(r => r.id === id && r.type === type && r.status === 'Pending');
                if (!req) return;

                req.status = 'Active';
                saveRequests(requests);
                saveAuditLog(`accept_${type}`, `Admin accepted ${type} request ID ${id}`);
                alert(`${type === 'managed-trading' ? 'Managed Trading Application' : 'Managed Account Request'} ID ${id} accepted! Status: Active.`);
                pendingModal.style.display = 'none';
                loadAllRequests();
                loadTraderDashboard();
            }

            function declineRequest(id) {
                console.log(`Declining request ID ${id}`);
                const requests = loadRequests();
                const req = requests.find(r => r.id === id && r.status === 'Pending');
                if (!req) return;

                req.status = 'Declined';
                saveRequests(requests);
                saveAuditLog(`decline_${req.type}`, `Admin declined ${req.type} request ID ${id}`);
                alert(`Request ID ${id} declined!`);
                pendingModal.style.display = 'none';
                loadAllRequests();
                loadTraderDashboard();
            }

            function loadAllRequests() {
                console.log('Loading all requests');
                const requests = loadRequests();
                allRequestsList.innerHTML = '';
                const status = filterStatus.value;
                const idFilter = filterId.value.toLowerCase();

                const filtered = requests.filter(req => {
                    const statusMatch = status === 'All' || req.status === status;
                    const idMatch = !idFilter || req.user_id?.toLowerCase().includes(idFilter) || req.trader_platform_id?.toLowerCase().includes(idFilter);
                    return statusMatch && idMatch;
                });

                if (filtered.length === 0) {
                    allRequestsList.innerHTML = '<li>No requests available.</li>';
                    return;
                }

                filtered.forEach(req => {
                    const li = document.createElement('li');
                    let text = `ID: ${req.id} | Type: ${req.type} | `;
                    if (req.type === 'co-funding') {
                        text += `Firm: ${req.prop_firm} | Size: ${req.account_size} | Price: $${req.account_price.toFixed(2)} | Profit Split: ${req.profit_split} | Requester Share: $${req.requester_share.toFixed(2)} | Co-Funder Share: $${req.co_funder_share.toFixed(2)} | User: ${req.user_id}`;
                    } else if (req.type === 'deposit') {
                        const method = req.method === 'Crypto' ? `Crypto (${req.details.coin_type || 'Unknown'})` : req.method;
                        text += `Amount: $${req.amount.toFixed(2)} | Method: ${method} | ${'proof' in req.details ? `Proof: ${req.details.proof}` : `Card: ${req.details.card_number}`} | User: ${req.user_id}`;
                    } else if (req.type === 'withdraw') {
                        const method = req.bank_name ? `${req.method} (${req.bank_name})` : req.method;
                        text += `Amount: $${req.amount.toFixed(2)} | Method: ${method} | Destination: ${req.destination} | User: ${req.user_id}`;
                    } else if (req.type === 'managed-trading') {
                        text += `Account: ${req.account_type} | Capital: $${req.capital_size.toFixed(2)} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | Withdrawal Terms: ${req.withdrawal_terms} | Lot Size: ${req.lot_size} | Experience: ${req.experience_level} | User: ${req.user_id}`;
                        if (req.risk_tolerance) text += ` | Risk Tolerance: ${req.risk_tolerance}`;
                        if (req.referral) text += ` | Referral: ${req.referral}`;
                        text += ` | Proof: ${req.proof_experience} (View) | Trades: ${req.proof_trades} (View) | Track Record: ${req.track_record} (View)`;
                        if (req.verification_link) text += ` | Verification: <a href="${req.verification_link}" target="_blank">Link</a>`;
                        if (req.social_media) text += ` | Social: <a href="${req.social_media}" target="_blank">Profile</a>`;
                    } else if (req.type === 'managed-account') {
                        text += `Broker: ${req.broker_name} | Account ID: ${req.account_id} | Trader ID: ${req.trader_platform_id} | Capital: $${req.capital_size.toFixed(2)} | Profit Split: ${req.profit_split} | Profit Target: ${req.profit_target}% | Return Cycle: ${req.return_cycle} | User: ${req.user_id}`;
                        if (req.risk_tolerance) text += ` | Risk Tolerance: ${req.risk_tolerance}`;
                        if (req.start_date) text += ` | Start Date: ${req.start_date}`;
                        if (req.duration) text += ` | Duration: ${req.duration}`;
                        if (req.account_proof) text += ` | Proof: ${req.account_proof} (View)`;
                        if (req.investor_password) text += ` | Password/API: ${req.investor_password}`;
                        if (req.status === 'Active' || req.status === 'Completed') {
                            const profit = req.capital_size * (req.profit_target / 100);
                            const [investorSplit, traderSplit] = req.profit_split.split('/').map(Number);
                            const investorProfit = profit * (investorSplit / (investorSplit + traderSplit));
                            const traderProfit = profit * (traderSplit / (investorSplit + traderSplit));
                            text += ` | Simulated Profit: $${profit.toFixed(2)} | Investor Share: $${investorProfit.toFixed(2)} | Trader Share: $${traderProfit.toFixed(2)}`;
                        }
                    }
                    text += ` | Status: ${req.status} | Timestamp: ${req.timestamp}`;
                    li.innerHTML = text;

                    if (req.status === 'Active') {
                        const completeButton = document.createElement('button');
                        completeButton.textContent = 'Mark as Completed';
                        completeButton.onclick = () => completeRequest(req.id);
                        const holdButton = document.createElement('button');
                        holdButton.textContent = 'Put on Hold';
                        holdButton.onclick = () => setStatus(req.id, 'On Hold');
                        const suspendButton = document.createElement('button');
                        suspendButton.textContent = 'Suspend';
                        suspendButton.onclick = () => setStatus(req.id, 'Suspended');
                        li.appendChild(completeButton);
                        li.appendChild(holdButton);
                        li.appendChild(suspendButton);
                    }
                    allRequestsList.appendChild(li);
                });
            }

            function setStatus(id, status) {
                console.log(`Setting request ID ${id} to status ${status}`);
                const requests = loadRequests();
                const req = requests.find(r => r.id === id && r.status === 'Active');
                if (!req) return;

                req.status = status;
                saveRequests(requests);
                saveAuditLog(`set_${status.toLowerCase().replace(' ', '_')}`, `Admin set request ID ${id} to ${status}`);
                alert(`Request ID ${id} set to ${status}!`);
                loadAllRequests();
                loadTraderDashboard();
            }

            function completeRequest(id) {
                console.log(`Completing request ID ${id}`);
                const requests = loadRequests();
                const req = requests.find(r => r.id === id && r.status === 'Active');
                if (!req) return;

                req.status = 'Completed';
                saveRequests(requests);
                saveAuditLog('complete_request', `Admin completed request ID ${id}`);
                alert(`Request ID ${id} marked as Completed!`);
                loadAllRequests();
                loadTraderDashboard();
            }

            function exportHistory() {
                console.log('Exporting agreement history');
                const requests = loadRequests();
                const data = JSON.stringify(requests, null, 2);
                const blob = new Blob([data], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const a = document.createElement('a');
                a.href = url;
                a.download = 'agreement_history.json';
                a.click();
                URL.revokeObjectURL(url);
                saveAuditLog('export_history', `Admin exported agreement history`);
            }

            // Messaging Functions
            function openMessagesModal() {
                console.log('Opening messages modal');
                messagesList.innerHTML = '';
                const messages = loadMessages();
                const userMessages = messages.filter(msg => msg.sender === userId || msg.recipient === userId);
                userMessages.forEach(msg => {
                    const div = document.createElement('div');
                    div.textContent = `${msg.timestamp} | ${msg.sender} to ${msg.recipient}: ${msg.text}`;
                    messagesList.appendChild(div);
                });
                if (userMessages.length === 0) messagesList.innerHTML = '<p>No messages.</p>';
                messageModal.style.display = 'flex';
            }

            function sendMessage() {
                console.log('Sending message');
                const recipient = messageRecipient.value;
                const text = messageText.value;
                if (!recipient || !text) {
                    alert('Please enter a recipient ID and message.');
                    return;
                }

                const messages = loadMessages();
                const newMessage = {
                    sender: userId,
                    recipient,
                    text,
                    timestamp: new Date().toLocaleString('en-US')
                };
                messages.push(newMessage);
                saveMessages(messages);
                saveAuditLog('send_message', `User ${userId} sent message to ${recipient}`);
                alert(`Message sent to ${recipient}!`);
                messageRecipient.value = '';
                messageText.value = '';
                openMessagesModal();
            }

            // Event Listeners
            if (firmSelect) firmSelect.addEventListener('change', updateAccountSizes);
            if (profitSplitInput) profitSplitInput.addEventListener('input', calculateShares);
            if (submitButton) submitButton.addEventListener('click', submitRequest);
            if (viewPendingButton) viewPendingButton.addEventListener('click', viewPending);
            if (closeModalButton) closeModalButton.addEventListener('click', () => {
                console.log('Closing pending modal');
                pendingModal.style.display = 'none';
            });
            if (submitDeposit) submitDeposit.addEventListener('click', submitDepositRequest);
            if (submitWithdraw) submitWithdraw.addEventListener('click', submitWithdrawRequest);
            if (submitManaged) submitManaged.addEventListener('click', submitManagedRequest);
            if (submitAccount) submitAccount.addEventListener('click', submitAccountRequest);
            if (openMessages) openMessages.addEventListener('click', openMessagesModal);
            if (sendMessage) sendMessage.addEventListener('click', sendMessage);
            if (closeMessageModal) closeMessageModal.addEventListener('click', () => {
                console.log('Closing message modal');
                messageModal.style.display = 'none';
            });
            if (applyFilter) applyFilter.addEventListener('click', loadAllRequests);

            tabs.forEach(tab => {
                tab.addEventListener('click', () => {
                    console.log(`Tab clicked: ${tab.dataset.tab}`);
                    tabs.forEach(t => t.classList.remove('active'));
                    tabContents.forEach(c => c.classList.remove('active'));
                    tab.classList.add('active');
                    document.getElementById(tab.dataset.tab).classList.add('active');
                });
            });

            depositMethodButtons.forEach(button => {
                button.addEventListener('click', () => {
                    console.log(`Deposit method clicked: ${button.dataset.method}`);
                    depositMethodButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    selectedDepositMethod = button.dataset.method;
                    updateDepositDetails();
                });
            });

            withdrawMethodButtons.forEach(button => {
                button.addEventListener('click', () => {
                    console.log(`Withdraw method clicked: ${button.dataset.method}`);
                    withdrawMethodButtons.forEach(btn => btn.classList.remove('active'));
                    button.classList.add('active');
                    selectedWithdrawMethod = button.dataset.method;
                    updateWithdrawDetails();
                });
            });

            if (bankNameSelect) {
                bankNameSelect.addEventListener('change', () => {
                    console.log('Bank name changed:', bankNameSelect.value);
                    bankNameCustom.style.display = bankNameSelect.value === 'Other' ? 'block' : 'none';
                });
            }

            if (profitSplit) {
                profitSplit.addEventListener('change', () => {
                    console.log('Profit split changed:', profitSplit.value);
                    customProfitSplit.style.display = profitSplit.value === 'Custom' ? 'block' : 'none';
                });
            }

            // Initial Load
            console.log('Initializing dashboard');
            updateAccountSizes();
            updateDepositDetails();
            updateWithdrawDetails();
            loadAllRequests();
            loadRecentTransactions();
            loadTraderDashboard();
            loadAuditLogs();
        });
    </script>
</body>
</html>
