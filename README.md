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
            background-image: url('backweb.jpg'); /* Replace with your background image */
            background-size: cover; /* Ensures the background image covers the entire viewport */
            background-attachment: fixed; /* Keeps the background image fixed in place */
            background-position: center; /* Centers the background image */
            background-repeat: no-repeat; /* Prevents the background image from repeating */
            color: white; /* Ensures text is readable on the background */
        }

        h1 {
            color: #ffffff; /* White text color for better readability */
            font-size: 50px;
            font-weight: bold;
            text-shadow: 2px 2px 8px rgba(0, 0, 0, 0.6); /* Adds a shadow to the text */
            background: linear-gradient(45deg, rgba(0, 0, 115, 0.8), rgba(255, 255, 255, 0.4)); /* Gradient background */
            padding: 20px;
            border-radius: 10px;
            display: inline-block;
            animation: fadeIn 3s ease-in-out; /* Fade-in animation */
        }

        h2 {
            color: #ffffff;
            font-size: 24px;
            margin: 10px 0;
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
            color: #ffffff;
        }

        #results p {
            background: rgba(200, 200, 224, 0.8); /* Semi-transparent background for better readability */
            padding: 10px;
            border-radius: 4px;
            text-align: left;
            color: black;
        }

        @keyframes fadeIn {
            from {
                opacity: 0;
            }
            to {
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <h1>Welcome to the Satellite tracker</h1>
    <h2>Using this website, you can find various satellites and space stations orbiting around us.</h2>
<style>
        
    h2 {
        color: black;
    }

    </style>
<!-- Sidebar with list of Indian satellites -->
<div class="sidebar">
    <h3>Indian Satellites</h3>

    <ul style="color: black; list-style-type: none; padding: 0;">
        <li>Aryabhata</li>
        <li>Bhaskara-I</li>
        <li>INSAT-3DR</li>
        <li>GSAT-19</li>
        <li>Cartosat-2</li>
        <li>IRNSS-1A</li>
        <li>Chandrayaan-2</li>
        <li>Astrosat</li>
        <li>NavIC</li>
        <li>RISAT-2</li>
    </ul>
</div>
    
    
    <div class="search-container">
        <input type="text" id="searchInput" placeholder="Enter Satellite Catalog Number...">
        <button onclick="searchSatellite()">Search</button>
    </div>
    <div id="results"></div>

    <script>
        // Function to search for a satellite using its Catalog Number
        function searchSatellite() {
            // Get the value entered in the search input field
            const query = document.getElementById('searchInput').value.trim();

            // Construct the URL for the CelesTrak API with the entered Catalog Number
            const url = `https://celestrak.com/NORAD/elements/gp.php?CATNR=${query}`;

            // Use the Fetch API to retrieve the TLE data for the specified satellite
            fetch(url)
                .then(response => response.text()) // Convert the response to text
                .then(data => {
                    // Check if data was returned
                    if (data) {
                        displayResults(data); // Display the results if data is present
                    } else {
                        // Display a message if no data was found for the specified Catalog Number
                        document.getElementById('results').innerHTML = '<p>No satellites found matching your search.</p>';
                    }
                })
                .catch(error => console.error('Error:', error)); // Log any errors in the console
        }

        // Function to display the satellite information in a more understandable format
        function displayResults(data) {
            // Select the 'results' div to display the output
            const resultsDiv = document.getElementById('results');
            resultsDiv.innerHTML = ''; // Clear any previous results

            // Split the retrieved TLE data into separate lines
            const lines = data.trim().split('\n');

            // Check if we have at least three lines of data (Name, TLE Line 1, TLE Line 2)
            if (lines.length >= 3) {
                const name = lines[0].trim(); // The first line is the satellite's name
                const tle1 = lines[1].trim(); // The second line is the first part of the TLE data
                const tle2 = lines[2].trim(); // The third line is the second part of the TLE data

                // Construct the HTML to display the satellite's name and key orbital elements
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
                // Add the constructed satellite info to the results div
                resultsDiv.innerHTML += satelliteInfo;
            } else {
                // If there are not enough lines of data, display a message indicating no data was found
                resultsDiv.innerHTML = '<p>No satellites found matching your search.</p>';
            }
        }

        // Helper function to extract and trim a specific part of the TLE data
        function getTLEPart(tle, start, end) {
            // Substring the TLE data between the specified start and end indices and trim any whitespace
            return tle.substring(start, end).trim();
        }
    </script>
</body>
</html>
