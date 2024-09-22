document.getElementById('jsonForm').addEventListener('submit', function (e) {
    e.preventDefault();

    const inputData = document.getElementById('inputData').value;
    const responseContainer = document.getElementById('responseContainer');
    const responseOptions = document.getElementById('responseOptions');

    // Simulated API Response
    const simulatedApiResponse = simulateApiResponse(inputData);

    if (simulatedApiResponse) {
        responseContainer.innerHTML = `<pre>${JSON.stringify(simulatedApiResponse, null, 2)}</pre>`;
        responseOptions.disabled = false; // Enable the dropdown
        responseOptions.value = ''; // Reset dropdown selection
    } else {
        responseContainer.innerHTML = '<p style="color: red;">Invalid JSON input.</p>';
        responseOptions.disabled = true; // Disable dropdown if input is invalid
    }
});

// Enable or disable submit button based on JSON validity
document.getElementById('inputData').addEventListener('input', function () {
    const submitButton = document.getElementById('submitButton');
    try {
        JSON.parse(this.value);
        submitButton.disabled = false;
    } catch (e) {
        submitButton.disabled = true;
    }
});

// Simulate API Response
function simulateApiResponse(input) {
    try {
        const parsedData = JSON.parse(input);
        const response = {
            is_success: true,
            user_id: "john_doe_17091999",
            email: "john@xyz.com",
            roll_number: "ABCD123",
            numbers: [],
            alphabets: [],
            highest_lowercase_alphabet: []
        };

        parsedData.data.forEach(item => {
            if (!isNaN(item)) {
                response.numbers.push(item);
            } else if (/[a-zA-Z]/.test(item)) {
                response.alphabets.push(item);
            }
        });

        // Find the highest lowercase alphabet
        const lowercaseAlphabets = response.alphabets.filter(char => char === char.toLowerCase());
        if (lowercaseAlphabets.length > 0) {
            response.highest_lowercase_alphabet = [lowercaseAlphabets.sort().pop()];
        }

        return response;
    } catch (e) {
        return null; // Return null if input is not valid JSON
    }
}

// Handle dropdown selection
document.getElementById('responseOptions').addEventListener('change', function () {
    const responseContainer = document.getElementById('responseContainer');
    const responseText = responseContainer.textContent;

    if (responseText) {
        const responseData = JSON.parse(responseText);
        let displayData = '';

        switch (this.value) {
            case 'alphabets':
                displayData = responseData.alphabets.length > 0 ? responseData.alphabets.join(', ') : 'No alphabets found.';
                break;
            case 'numbers':
                displayData = responseData.numbers.length > 0 ? responseData.numbers.join(', ') : 'No numbers found.';
                break;
            case 'highestLowercase':
                displayData = responseData.highest_lowercase_alphabet.length > 0 ? responseData.highest_lowercase_alphabet.join(', ') : 'No lowercase letters found.';
                break;
            default:
                displayData = '';
        }

        responseContainer.innerHTML = `<pre>${displayData}</pre>`;
    }
});

