# HACKATHON
Satellite searching 
//website construction 
<h1 style="color: rgb(0, 0, 115);;font-size:50px;text-align:center;"> welcome to the space station discoverer </h1>
<h2 style="text-align:center;">Using this website you can find various satellites and space stations roaming aroun us in an orbital.</h2>
<img src="spaceimage.jpg" style="width:800px;height:600px;">
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ISS Tracker</title>
</head>
<body>
    <h1>Current Location of the ISS</h1>
    <p id="iss-location">Loading...</p>

    <script src="script.js"></script>
</body>
</html>
// API URL for the ISS current location
const apiUrl = 'http://api.open-notify.org/iss-now.json';

// Function to fetch and display the ISS location
async function fetchISSLocation() {
    try {
        const response = await fetch();
        const data = await response.json();
        
        const latitude = data.iss_position.latitude;
        const longitude = data.iss_position.longitude;

        document.getElementById('iss-location').textContent = 
            `Latitude: ${latitude}, Longitude: ${longitude}`;
    } catch (error) {
        console.error('Error fetching the ISS location:', error);
        document.getElementById('iss-location').textContent = 
            'Failed to load the ISS location.';
    }
}
function searchSatellite() {
    const satelliteName = document.getElementById('satelliteName').value;
    
    if (!satelliteName) {
        alert('Please enter a satellite name.');
        return;
    }
    
    const apiUrl = `https://api.example.com/satellite?name=${satelliteName}`;
    
    fetch(apiUrl)
        .then(response => response.json())
        .then(data => {
            displaySatelliteInfo(data);
            loadSatelliteMap(data.position);
        })
        .catch(error => {
            console.error('Error:', error);
            alert('Satellite not found or an error occurred.');
        });
}

function displaySatelliteInfo(data) {
    const infoDiv = document.getElementById('satelliteInfo');
    infoDiv.innerHTML = `
        <h2>${data.name}</h2>
        <p>Latitude: ${data.position.latitude}</p>
        <p>Longitude: ${data.position.longitude}</p>
        <p>Altitude: ${data.position.altitude} km</p>
    `;
}

function loadSatelliteMap(position) {
    const mapDiv = document.getElementById('satelliteMap');
    const mapUrl = `https://maps.google.com/maps?q=${position.latitude},${position.longitude}&z=4&output=embed`;
    mapDiv.innerHTML = `<iframe width="100%" height="100%" src="${mapUrl}"></iframe>`;
}


