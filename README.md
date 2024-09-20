# plandora
calendar api
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Google Calendar Events</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 20px;
        }
        ul {
            list-style-type: none;
            padding: 0;
        }
        li {
            margin: 5px 0;
            padding: 10px;
            border: 1px solid #ccc;
        }
    </style>
    <script src="https://apis.google.com/js/api.js"></script>
</head>
<body>

<h2>Upcoming Events</h2>
<ul id="eventList"></ul>

<script>
    const API_KEY = AIzaSyANYOwd8b9qyIrPV1OnPki6jKW_mL036_A; // Replace with your API key
    const CLIENT_ID = 976649972416-90so6n6u934246tl1t1nd8v2btipthvu.apps.googleusercontent.com; // Replace with your Client ID
    const CALENDAR_ID = 'primary'; // Use 'primary' for the user's primary calendar

    function handleClientLoad() {
        gapi.load('client:auth2', initClient);
    }

    function initClient() {
        gapi.client.init({
            apiKey: API_KEY,
            clientId: CLIENT_ID,
            discoveryDocs: ["https://www.googleapis.com/discovery/v1/apis/calendar/v3/rest"],
            scope: "https://www.googleapis.com/auth/calendar.readonly"
        }).then(() => {
            listUpcomingEvents();
        }).catch(error => {
            console.error("Error initializing Google API client:", error);
        });
    }

    function listUpcomingEvents() {
        gapi.client.calendar.events.list({
            'calendarId': CALENDAR_ID,
            'timeMin': (new Date()).toISOString(),
            'maxResults': 10,
            'singleEvents': true,
            'orderBy': 'startTime'
        }).then(response => {
            console.log(response); // Log the entire response
            const events = response.result.items;
            const eventList = document.getElementById('eventList');
            eventList.innerHTML = ''; // Clear previous events

            if (events.length) {
                events.forEach(event => {
                    const when = event.start.dateTime || event.start.date;
                    const listItem = document.createElement('li');
                    listItem.textContent = `${event.summary} (${when})`;
                    eventList.appendChild(listItem);
                });
            } else {
                eventList.textContent = 'No upcoming events found.';
            }
        }).catch(error => {
            console.error("Error fetching events:", error);
        });
    }

    // Load the API client and auth2 library
    handleClientLoad();
</script>

</body>
</html>

