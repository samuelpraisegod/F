<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Test Dashboard</title>
    <style>
        body { font-family: Arial, sans-serif; margin: 0; padding: 0; }
        header { background: #333; color: white; padding: 10px; display: flex; justify-content: space-between; }
        #hamburger { cursor: pointer; font-size: 24px; z-index: 20; }
        #menu { display: none; position: absolute; top: 50px; left: 10px; background: white; border: 1px solid #ddd; padding: 10px; z-index: 15; }
        #menu ul { list-style: none; padding: 0; }
        #menu li { margin-bottom: 10px; cursor: pointer; }
        #main-content { max-width: 600px; margin: 20px auto; padding: 20px; }
        section { display: none; }
        #managed-trading-section { display: block; }
        label { display: block; margin-bottom: 5px; }
        input { width: 100%; padding: 8px; margin-bottom: 10px; box-sizing: border-box; }
        button { padding: 10px 20px; cursor: pointer; z-index: 10; position: relative; background: blue; color: white; }
        @media (max-width: 600px) { button { width: 100%; } }
    </style>
</head>
<body>
    <header>
        <div id="hamburger">&#9776;</div>
        <h1>Test Dashboard</h1>
    </header>
    <nav id="menu">
        <ul>
            <li data-section="managed-trading-section">Managed Trading Application</li>
        </ul>
    </nav>
    <div id="main-content">
        <section id="managed-trading-section">
            <h1>Managed Trading Application</h1>
            <label for="trader-capital-size">Capital Size ($):</label>
            <input type="number" id="trader-capital-size" min="1000" placeholder="Enter amount (min $1,000)">
            <button id="submit-managed">Submit Application</button>
        </section>
    </div>
    <script>
        document.addEventListener('DOMContentLoaded', () => {
            console.log('DOM loaded, initializing test dashboard...');
            const hamburger = document.getElementById('hamburger');
            const menu = document.getElementById('menu');
            const menuItem = document.querySelector('li[data-section="managed-trading-section"]');
            const section = document.getElementById('managed-trading-section');
            const submitButton = document.getElementById('submit-managed');

// Debug all button clicks
            document.querySelectorAll('button').forEach(btn => {
                btn.addEventListener('click', () => {
                    console.log(`Button clicked: ${btn.id || btn.textContent}`);
                });
            });

 // Hamburger menu toggle
            if (hamburger && menu) {
                hamburger.addEventListener('click', () => {
                    console.log('Hamburger menu clicked');
                    menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
                });
            } else {
                console.error('Hamburger or menu not found');
            }

 // Menu item navigation
            if (menuItem && section) {
                menuItem.addEventListener('click', () => {
                    console.log('Menu item clicked: managed-trading-section');
                    section.style.display = 'block';
                    menu.style.display = 'none';
                });
            } else {
                console.error('Menu item or section not found');
            }

 // Submit button
            if (submitButton) {
                submitButton.addEventListener('click', () => {
                    console.log('Submit Managed Trading clicked');
                    const capital = document.getElementById('trader-capital-size').value;
                    if (!capital || capital < 1000) {
                        alert('Please enter a valid capital size (minimum $1,000).');
                        return;
                    }
                    alert('Application submitted successfully!');
                });
            } else {
                console.error('Submit button not found');
            }
        });
    </script>
</body>
</html>
