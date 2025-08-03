<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Calculator - Your Math Assistant</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Hind+Siliguri:wght@400;600;700&display=swap');
        
        body {
            font-family: 'Hind Siliguri', sans-serif;
            transition: background-color 0.3s;
        }
        
        .btn {
            transition: all 0.2s;
        }
        
        .btn:hover {
            transform: scale(1.05);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        .btn:active {
            transform: scale(0.98);
        }
        
        .display {
            min-height: 100px;
            transition: all 0.3s;
        }
        
        .bangla-num {
            font-family: 'Hind Siliguri', sans-serif;
            font-feature-settings: "numr";
        }
    </style>
</head>
<body class="bg-gray-100 min-h-screen flex items-center justify-center p-4">
    <div class="w-full max-w-md mx-auto">
        <div class="text-center mb-4">
            <h1 class="text-3xl font-bold text-indigo-700">CALCULATION FOR STUDY</h1>
            <p class="text-gray-600">YOUR MATH ASSISTANT CREATED BY RIJU AI



</p>
        </div>
        
        <div class="bg-white rounded-2xl shadow-xl overflow-hidden">
            <div class="p-5 bg-gray-800 text-right">
                <div id="history" class="text-gray-300 text-sm h-6 overflow-hidden"></div>
                <div id="display" class="text-white text-4xl font-semibold mt-2 display"></div>
            </div>
            
            <div class="grid grid-cols-4 gap-3 p-5">
                <button onclick="clearAll()" class="btn bg-red-500 hover:bg-red-600 text-white font-bold py-4 rounded-lg col-span-2">C</button>
                <button onclick="backspace()" class="btn bg-gray-300 hover:bg-gray-400 p-4 rounded-lg">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6 mx-auto" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m-6-6h6a2 2 0 012 2v8a2 2 0 01-2 2H8a2 2 0 01-2-2v-8a2 2 0 012-2z" />
                    </svg>
                </button>
                <button onclick="appendOperator('/')" class="btn bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-4 rounded-lg">÷</button>
                
                <button onclick="appendNumber('7')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">7</button>
                <button onclick="appendNumber('8')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">8</button>
                <button onclick="appendNumber('9')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">9</button>
                <button onclick="appendOperator('*')" class="btn bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-4 rounded-lg">×</button>
                
                <button onclick="appendNumber('4')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">4</button>
                <button onclick="appendNumber('5')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">5</button>
                <button onclick="appendNumber('6')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">6</button>
                <button onclick="appendOperator('-')" class="btn bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-4 rounded-lg">-</button>
                
                <button onclick="appendNumber('1')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">1</button>
                <button onclick="appendNumber('2')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">2</button>
                <button onclick="appendNumber('3')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">3</button>
                <button onclick="appendOperator('+')" class="btn bg-indigo-500 hover:bg-indigo-600 text-white font-bold py-4 rounded-lg">+</button>
                
                <button onclick="toggleNumberFormat()" id="formatToggle" class="btn bg-gray-300 hover:bg-gray-400 font-bold py-4 rounded-lg col-span-1">ENG</button>
                <button onclick="appendNumber('0')" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">0</button>
                <button onclick="appendDecimal()" class="btn bg-gray-200 hover:bg-gray-300 font-bold py-4 rounded-lg">.</button>
                <button onclick="calculate()" class="btn bg-green-500 hover:bg-green-600 text-white font-bold py-4 rounded-lg">=</button>
            </div>
        </div>
        
        <div class="mt-6 text-center text-gray-600 text-sm">
            <p>Perform calculations easily | Ready for your math work</p>
        </div>
    </div>
    
    <script>
        let currentInput = '0';
        let previousInput = '';
        let operation = null;
        let resetInput = false;
        let showBanglaNumbers = false;
        
        const display = document.getElementById('display');
        const history = document.getElementById('history');
        const formatToggle = document.getElementById('formatToggle');
        
        function updateDisplay() {
            let displayValue = currentInput;
            
            formatToggle.style.display = 'none'; // Hide the format toggle button
            
            display.textContent = displayValue;
        }
        
        function appendNumber(number) {
            if (currentInput === '0' || resetInput) {
                currentInput = number;
                resetInput = false;
            } else {
                currentInput += number;
            }
            updateDisplay();
        }
        
        function appendDecimal() {
            if (resetInput) {
                currentInput = '0.';
                resetInput = false;
            } else if (!currentInput.includes('.')) {
                currentInput += '.';
            }
            updateDisplay();
        }
        
        function appendOperator(op) {
            if (operation !== null) calculate();
            previousInput = currentInput;
            operation = op;
            resetInput = true;
            history.textContent = `${previousInput} ${operation}`;
        }
        
        function calculate() {
            if (operation === null || resetInput) return;
            
            let result;
            const prev = parseFloat(previousInput);
            const current = parseFloat(currentInput);
            
            switch (operation) {
                case '+':
                    result = prev + current;
                    break;
                case '-':
                    result = prev - current;
                    break;
                case '*':
                    result = prev * current;
                    break;
                case '/':
                    if (current === 0) {
                        result = 'Error';
                        break;
                    }
                    result = prev / current;
                    break;
                default:
                    return;
            }
            
            history.textContent = `${previousInput} ${operation} ${currentInput} =`;
            currentInput = result.toString();
            operation = null;
            resetInput = true;
            updateDisplay();
        }
        
        function clearAll() {
            currentInput = '0';
            previousInput = '';
            operation = null;
            history.textContent = '';
            updateDisplay();
        }
        
        function backspace() {
            if (currentInput.length === 1 || (currentInput.length === 2 && currentInput.startsWith('-'))) {
                currentInput = '0';
            } else {
                currentInput = currentInput.slice(0, -1);
            }
            updateDisplay();
        }
        
        function toggleNumberFormat() {
            showBanglaNumbers = !showBanglaNumbers;
            updateDisplay();
        }
        
        // Removed bangla number conversion function since it's not needed
        
        // Initialize display
        updateDisplay();
        
        // Keyboard support
        document.addEventListener('keydown', function(event) {
            if (event.key >= '0' && event.key <= '9') appendNumber(event.key);
            else if (event.key === '.') appendDecimal();
            else if (event.key === '+') appendOperator('+');
            else if (event.key === '-') appendOperator('-');
            else if (event.key === '*') appendOperator('*');
            else if (event.key === '/') appendOperator('/');
            else if (event.key === 'Enter' || event.key === '=') calculate();
            else if (event.key === 'Escape') clearAll();
            else if (event.key === 'Backspace') backspace();
        });
    </script>
</body>
</html>

