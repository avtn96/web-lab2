document.addEventListener('DOMContentLoaded', function () {
    const display = document.getElementById('display');

    // update the display
    function updateDisplay(value) {
        display.textContent = value;
    }

    // calculate n!
    function factorial(n) {
        let result = 1;
        for (let i = 2; i <= n; i++) {
            result *= i;
        }
        return result;
    }

function evaluateExpression(input) {
    input = input.replace(/π/g, 'Math.PI')
                 .replace(/e/g, 'Math.E')
                 .replace(/1\/x/g, '1/')
                 .replace(/x\^2/g, '**2')
                 .replace(/√x/g, (match) => match.replace('√x', 'Math.sqrt'))
                 .replace(/x\^y/g, '**') // Handling exponentiation
                 .replace(/\|x\|/g, 'Math.abs')
                 .replace(/exp/g, 'Math.exp')
                 .replace(/10\^x/g, '10**')
                 .replace(/log/g, 'Math.log10')
                 .replace(/ln/g, 'Math.log')
                 .replace(/mod/g, '%');

    input = input.replace(/(\d+)!/g, (_, n) => factorial(parseInt(n)));

    try {
        return new Function(`return (${input});`)();
    } catch (error) {
        console.error('Error evaluating expression:', error);
        return 'Error';
    }
}


    function appendChar(char) {
        if (display.textContent === '0' || display.textContent === 'Error') {
            updateDisplay(char);
        } else {
            updateDisplay(display.textContent + char);
        }
    }

    // clear the display
    function clearDisplay() {
        updateDisplay('0');
    }

    // handle the '=' 
    function calculateResult() {
        const result = evaluateExpression(display.textContent);
        updateDisplay(result.toString());
    }

    // Add click event listeners to all buttons
    document.querySelectorAll('button').forEach(button => {
        button.addEventListener('click', () => {
            const value = button.textContent;

            switch (value) {
                case 'C':
                    clearDisplay();
                    break;
                case '=':
                    calculateResult();
                    break;
                default:
                    appendChar(value);
                    break;
            }
        });
    });
});
