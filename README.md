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
            text-align: left;
        }
    </style>
</head>
<body>
    <h1 style="color: rgb(0, 0, 115); font-size: 50px;">Welcome to the Space Station Discoverer</h1>
    <h2>Using this website, you can find various satellites and space stations orbiting around us.</h2>
    <img src="spaceimage.jpg" style="width: 800px; height: 600px;">
    
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Enter Satellite Catalog Number...">
        <button onclick="searchSatellite()">Search</button>
    </div>
    <div id="results"></div>

    <script>
        function searchSatellite() {
            const query = document.getElementById('searchInput').value.trim();
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
                    <h2>Satellite Name: ${name}</h2>
                    <p><strong>Two-Line Element Set (TLE):</strong></p>
                    <p><strong>TLE Line 1:</strong> ${tle1}</p>
                    <p><strong>TLE Line 2:</strong> ${tle2}</p>
                    <p><strong>Key Orbital Elements:</strong></p>
                    <ul>
                        <li><strong>Inclination:</strong> ${getTLEPart(tle2, 8, 16)} degrees</li>
                        <li><strong>Right Ascension of Ascending Node:</strong> ${getTLEPart(tle2, 17, 25)} degrees</li>
                        <li><strong>Eccentricity:</strong> 0.${getTLEPart(tle2, 26, 33)}</li>
                        <li><strong>Argument of Perigee:</strong> ${getTLEPart(tle2, 34, 42)} degrees</li>
                        <li><strong>Mean Anomaly:</strong> ${getTLEPart(tle2, 43, 51)} degrees</li>
                        <li><strong>Mean Motion:</strong> ${getTLEPart(tle2, 52, 63)} revs per day</li>
                        <li><strong>Revolution Number at Epoch:</strong> ${getTLEPart(tle2, 63, 68)}</li>
                    </ul>
                `;
                resultsDiv.innerHTML += satelliteInfo;
            } else {
                resultsDiv.innerHTML = '<p>No satellites found matching your search.</p>';
            }
        }

        function getTLEPart(tle, start, end) {
            return tle.substring(start, end).trim();
        }
    </script>
</body>
</html>

       
