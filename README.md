# HACKATHON
Satellite searching 
//website construction 
<h1 style="color: rgb(0, 0, 115);;font-size:50px;text-align:center;"> welcome to the space station discoverer </h1>
<h2 style="text-align:center;">Using this website you can find various satellites and space stations roaming aroun us in an orbital.</h2>
<img src="spaceimage.jpg" style="width:800px;height:600px;">
<h1 style="color: rgb(0, 0, 115);;font-size:50px;text-align:center;"> welcome to the space station discoverer </h1>
<h2 style="text-align:center;">Using this website you can find various satellites and space stations roaming aroun us in an orbital.</h2>
<img src="spaceimage.jpg" style="width:800px;height:600px;">
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>NASA Satellite Tracker</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f4;
        }

        .search-container {
            display: flex;
            align-items: center;
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
            max-width: 600px;
        }

        #results p {
            background: #e0e0e0;
            padding: 10px;
            border-radius: 4px;
        }
    </style>
</head>
<body>
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Enter Satellite Name...">
        <button onclick="searchSatellite()">Search</button>
    </div>
    <div id="results"></div>

    <script>
        function searchSatellite() {
            const query = document.getElementById('searchInput').value;
            const apiKey = 'q4dODOUAu8BuY5vX7ef5zoCP59dR6c1jaFYPKwJt'; 
            const url = `https://api.nasa.gov/neo/rest/v1/neo/browse?api_key=${apiKey}&s=${query}`;

            fetch(url)
                .then(response => response.json())
                .then(data => {
                    displayResults(data);
                })
                .catch(error => console.error('Error:', error));
        }

        function displayResults(data) {
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = '';

            if (data.near_earth_objects && data.near_earth_objects.length > 0) {
                data.near_earth_objects.forEach(satellite => {
                    const satelliteInfo = `
                        <h2>${satellite.name}</h2>
                        <p>ID: ${satellite.id}</p>
                        <p>NASA JPL URL: <a href="${satellite.nasa_jpl_url}" target="_blank">${satellite.nasa_jpl_url}</a></p>
                        <p>Magnitude: ${satellite.absolute_magnitude_h}</p>
                    `;
                    resultsDiv.innerHTML += satelliteInfo;
                });
            } else {
                resultsDiv.innerHTML = '<p>No satellites found.</p>';
            }
        }
    </script>
</body>
</html>


