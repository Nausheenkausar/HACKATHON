# HACKATHON
Satellite searching 
//website construction 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Satellite Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            text-align: center;
            background-color: #f4f4f4;
        }

        .search-container {
            display: inline-block;
            margin-top: 20px;
        }

        input[type="text"] {
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 4px;
            width: 300px;
        }

        button {
            padding: 10px 15px;
            font-size: 16px;
            margin-left: 10px;
            border: none;
            background-color: #007BFF;
            color: white;
            border-radius: 4px;
            cursor: pointer;
        }

        button:hover {
            background-color: #0056b3;
        }

        #results {
            margin-top: 20px;
            width: 100%;
            max-width: 800px;
            margin: 0 auto;
        }

        #results h2 {
            color: #333;
        }

        #results p {
            background: #e0e0e0;
            padding: 10px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <h1 style="color: rgb(0, 0, 115); font-size: 50px;">Welcome to the Space Station Discoverer</h1>
    <h2>Using this website, you can find various satellites and space stations orbiting around us.</h2>
    <img src="spaceimage.jpg" style="width: 800px; height: 600px;">
    
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Enter Satellite Catalogue no...">
        <button onclick="searchSatellite()">Search</button>
    </div>
    <div id="results"></div>

    <script>
        function searchSatellite() {
            const query = document.getElementById('searchInput').value.toLowerCase();
            const url = `https://celestrak.com/NORAD/elements/gp.php?CATNR=${query}`;

            fetch(url)
                .then(response => response.text())
                .then(data => {
                    if (data) {
                        displayResults(data);
                    } else {
                        document.getElementById('results').innerHTML = '<p>No satellites found matching your search.</p>';
                    }
                })
                .catch(error => console.error('Error:', error));
        }

        function displayResults(data) {
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';

            const lines = data.trim().split('\n');

            if (lines.length >= 3) {
                const name = lines[0].trim();
                const tle1 = lines[1].trim();
                const tle2 = lines[2].trim();

                const satelliteInfo = `
                    <h2>${name}</h2>
                    <p>TLE Line 1: ${tle1}</p>
                    <p>TLE Line 2: ${tle2}</p>
                `;
                resultsDiv.innerHTML += satelliteInfo;
            } else {
                resultsDiv.innerHTML = '<p>No satellites found matching your search.</p>';
            }
        }
    </script>
</body>
</html>

      
            
   
              
              
