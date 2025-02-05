# CodeHackersProj

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Skin Disease Prediction</title>
    <style>
        /* Basic Reset */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: url('https://st.depositphotos.com/3405213/4937/i/450/depositphotos_49377941-stock-photo-young-doctor.jpg') no-repeat center center fixed;
            background-size: cover;
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            align-items: center;
            color: #333;
        }

        header {
            text-align: center;
            padding: 40px 20px;
            margin-top: 20px;
        }

        .logo {
            border-radius: 50%;
        }

        h1 {
            font-size: 2.5rem;
            font-weight: 600;
            color: #030303;
        }

        section {
            width: 100%;
            max-width: 900px;
            margin: 30px auto;
            padding: 20px;
            background: rgba(255, 255, 255, 0.8);
            border-radius: 15px;
            box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
        }

        label,
        input,
        select {
            display: block;
            margin: 10px 0;
            padding: 10px;
            font-size: 1rem;
            width: 100%;
            border-radius: 8px;
            border: 1px solid #ccc;
        }

        button {
            padding: 12px 20px;
            background: #4CAF50;
            color: white;
            border: none;
            font-size: 1.2rem;
            border-radius: 8px;
            cursor: pointer;
            transition: background 0.3s ease;
        }

        button:hover {
            background: #45a049;
        }

        #predictionResult {
            font-size: 1.2rem;
            color: #333;
            margin-top: 20px;
            line-height: 1.8;
        }

        .success {
            color: #4CAF50;
            font-weight: bold;
        }

        .error {
            color: #FF0000;
            font-weight: bold;
        }

        .processing {
            color: #FFA500;
            font-weight: bold;
        }

        .highlight {
            font-weight: bold;
            color: #007BFF;
        }
    </style>
</head>

<body>
    <header>
        <div class="container">
            <img src="https://cdn.sanity.io/images/kts928pd/production/acf71dc493554cc492578b8b5b8beb4ee20e8873-731x731.png"
                height="100" width="100" alt="Healthy Life Logo" class="logo">
            <h1>Skin Disease Diagnosis</h1>
            <p>Upload a skin image for diagnosis</p>
        </div>
    </header>

    <section id="upload-section">
        <div class="container">
            <h2>Enter Your Details & Upload Skin Image:</h2>
            <form id="imageForm" enctype="multipart/form-data">
                <label for="name">Name:</label>
                <input type="text" id="name" name="name" placeholder="Enter your name" required>

                <label for="age">Age:</label>
                <input type="number" id="age" name="age" placeholder="Enter your age" required>

                <label for="gender">Gender:</label>
                <select id="gender" name="gender" required>
                    <option value="" disabled selected>Select Gender</option>
                    <option value="Male">Male</option>
                    <option value="Female">Female</option>
                    <option value="Other">Other</option>
                </select>

                <h3>Upload Your Skin Image:</h3>
                <input type="file" id="skinImage" name="skinImage" accept="image/*" required>
                <button type="submit">Upload and Diagnose</button>
            </form>
        </div>
    </section>

    <section id="result-section">
        <div class="container">
            <h2>Diagnosis Result:</h2>
            <p id="predictionResult" class="processing">Please upload an image to get a diagnosis.</p>
        </div>
    </section>

    <script>
        const form = document.getElementById('imageForm');
        const resultSection = document.getElementById('result-section');

        form.addEventListener('submit', function (event) {
            event.preventDefault();

            const predictionResult = document.getElementById('predictionResult');
            predictionResult.textContent = 'Diagnosing... Please wait.';
            predictionResult.className = 'processing';

            const formData = new FormData(form);

            fetch('https://6283-35-185-215-133.ngrok-free.app/predict', {
                method: 'POST',
                body: formData
            })
                .then(response => {
                    if (!response.ok) throw new Error('Error in the response.');
                    return response.json();
                })
                .then(data => {
                    if (data.error) {
                        predictionResult.textContent = `Error: ${data.error}`;
                        predictionResult.className = 'error';
                    } else {
                        predictionResult.innerHTML = `
                            <span class="success">Diagnosis Successful:</span><br>
                            <span class="highlight">Name:</span> ${data.name}<br>
                            <span class="highlight">Age:</span> ${data.age}<br>
                            <span class="highlight">Gender:</span> ${data.gender}<br>
                            <span class="highlight">Prediction:</span> ${data.prediction}<br>
                            <span class="highlight">Description:</span> ${data.description}
                        `;
                        predictionResult.className = 'success';
                    }
                })
                .catch(error => {
                    predictionResult.textContent = `Error: ${error.message}`;
                    predictionResult.className = 'error';
                });
        });
    </script>
</body>

</html>
