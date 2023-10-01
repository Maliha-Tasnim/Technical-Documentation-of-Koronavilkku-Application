# Technical-Documentation-of-Koronavilkku-Application

The aim of this technical documentation is to present a plausible architectural description for the Koronavilkku++ application. This document serves as a component in modeling the application, outlining its operational contexts and the necessary connections to external systems, such as the THL service. It was made in conjunction with the Corona tracker assignment for the 'COMP.SE.210-2020-2021-1 Large Scale Software Design course at Tampere University.

This document outlines the comprehensive structure of a new and more resilient COVID-19 tracking solution, addressing the increasing demand for containment in light of the worsening national and global circumstances. The documentation employs the 4+1 view model to elucidate the architecture of Koronavilkku++.

The functional requirements encompass all the features and functionalities that Koronavilkku++ should provide. These requirements are categorized into subsections based on their respective features. The full implementation of these described features is of utmost importance. However, the specific instructions for implementation can be adjusted to enhance the system's design. The functional, non-functional and architectural requirements are as below:

*** Registration of a new user
To use the app, the user must use strong authentication which creates them a personal user id.
1. User identifies themselves to the server with Mobile Id or Bank Id.
2. After identification, the server creates a unique user id for the user and transmits it to the mobile app.
3. The app asks the user permission to use Bluetooth and location tracking. The user must accept these to keep using the app.
4. USER_ID is used in all communication with the app instead of the user data.

*** Checking for exposures
The app checks for exposures by comparing its encounters with a list of infected user ids provided by the server.
1. App requests for information about new infections from the server twice a day and every time the mobile is turned on.
2. The server responds the app requesting for information with the infected user ids and infection dates.
3. App checks if it has a registered encounter with any of the user ids on the list within 3 days.
4. If the app detects an exposure, the app sends information back to the server and informs the user about the exposure.
5. In case of an exposure, the server updates the user’s user id status to exposed.
6. The app will send the infected user’s location log from the last three days to the server.
7. The server shares the location logs of infected users to the THL service.


*** Mandatory notifications about infections
The user’s health status changes are updated by health care professionals and stored on the server. The app must be used by all Finnish residents.
1. A health care professional must report infections to the THL service. The status of the USER_ID status is updated to infected on the server.
2. A health care professional must report negative test cases to the server. The status of the USER_ID status is updated to healthy on the server.
3. The server updates the statuses of user ids from infected to healthy 21 days after the confirmation of infection and stores all confirmed infections.
4. The app ensures that the user has Bluetooth on.


*** Proximity tracking
Being in proximity with another device is stored as an encounter if it lasts longer than 2 minutes.
1. The app sends its user id repeatedly to all nearby devices via Bluetooth.
2. The app listens to incoming user ids from nearby devices.
3. The app tracks the start time of the encounter of two devices within a range of 10 meters.
4. If the encounter lasts for longer than 2 minutes, the app will store the encounter.
5. The saved encounters will be deleted after 21 days.
6. Location is saved for the start and end of the encounters.
7. If a user has encounters while on a site, the site information is stored with the encounter.


*** Location tracking of exposed and infected users
Sites can be used to deny the access to the site for non-healthy users.
1. Sites can require users to authenticate at the entrance with the app with a QR code.
2. If a user attempts to enter a site, the site reads the user id from the app, authenticates itself as an authorized Site to a THL service and inquires the health state of the user.
3. If the authentication shows the user as healthy, the entrance is permitted.

*** Non-functional requirements
1. Site does not have to be an authorized site and can be a geolocation or a Bluetooth ID of device.
2. Site does not need to be statically bound to one location and can be e.g., a moving bus or a train cabin.


*** Architectural requirements
1. The app must be supported on Android and iOS devices.