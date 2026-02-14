# Requirements Document: SafeRoute

## Introduction

SafeRoute is a public transport and safety companion app designed to help women and vulnerable groups find safer public transport routes. The system combines real-time safety data, AI risk scoring, and voice-guided navigation to provide users with informed route choices before they step out. The app integrates multiple data sources including crime statistics, crowdsourced safety reports, lighting conditions, and crowd density to generate safety scores for route segments and provide clear, non-alarming guidance.

## Glossary

- **SafeRoute_System**: The complete mobile application and backend services that provide route planning with safety scoring
- **User**: A person using the SafeRoute mobile application to plan or navigate routes
- **Route_Segment**: A portion of a journey (bus ride, metro ride, or walking path) between two points
- **Safety_Score**: A numerical rating (0-100) indicating the relative safety of a route segment based on multiple factors
- **Safety_Mode**: An active monitoring state where the system tracks user location and provides real-time alerts
- **Risk_Scoring_Engine**: The AI component that analyzes safety data and calculates safety scores
- **Help_Point**: A verified location with emergency assistance (police station, hospital, safe space)
- **Trusted_Contact**: A user-designated person who can receive live location updates during Safety Mode
- **Route_Deviation**: When user's actual location differs from planned route by more than 50 meters
- **SOS_Button**: One-tap emergency alert mechanism that notifies trusted contacts and authorities
- **Admin_Dashboard**: Web interface for administrators and authorities to monitor safety data
- **Incident_Report**: Anonymous user-submitted information about unsafe conditions or events
- **Safety_Rating**: User-friendly categorization of safety scores (Safer, Moderate, Less Safe)
- **Voice_Assistant**: Speech-based interface for route input and navigation guidance
- **Multi_Modal_Route**: A journey combining different transport types (bus, metro, walking)

## Requirements

### Requirement 1: User Registration and Authentication

**User Story:** As a new user, I want to register for SafeRoute using my phone or email, so that I can access personalized safety features and save my preferences.

#### Acceptance Criteria

1. WHEN a user provides a valid phone number or email address, THE SafeRoute_System SHALL create a new user account
2. WHEN a user provides an invalid phone number or email format, THE SafeRoute_System SHALL reject the registration and display a descriptive error message
3. WHEN a user completes registration, THE SafeRoute_System SHALL send a verification code to the provided contact method
4. WHEN a user enters a valid verification code within 10 minutes, THE SafeRoute_System SHALL activate the account
5. WHEN a user enters an invalid verification code, THE SafeRoute_System SHALL reject the verification and allow up to 3 retry attempts

### Requirement 2: User Profile and Preferences

**User Story:** As a user, I want to set my transport preferences and accessibility needs, so that the system provides routes tailored to my requirements.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL allow users to select preferred transport modes from the set {bus, metro, train, walking}
2. THE SafeRoute_System SHALL allow users to enable accessibility options including wheelchair-friendly routes and well-lit paths
3. THE SafeRoute_System SHALL allow users to select between voice-first mode and silent mode
4. WHEN a user updates profile preferences, THE SafeRoute_System SHALL persist the changes immediately
5. WHEN generating routes, THE SafeRoute_System SHALL filter and prioritize options based on user preferences

### Requirement 3: Location Detection and Context Awareness

**User Story:** As a user, I want the app to automatically detect my location and current time context, so that I receive relevant safety information without manual input.

#### Acceptance Criteria

1. WHEN the app launches, THE SafeRoute_System SHALL request GPS location permission from the user
2. WHEN location permission is granted, THE SafeRoute_System SHALL determine the user's current coordinates within 20 meters accuracy
3. THE SafeRoute_System SHALL detect the current time of day and classify it as {morning, afternoon, evening, night}
4. THE SafeRoute_System SHALL detect the day type as {weekday, weekend, holiday}
5. THE SafeRoute_System SHALL allow users to manually set a future time for advance route planning
6. WHEN location services are unavailable, THE SafeRoute_System SHALL allow manual location entry via address search

### Requirement 4: Route Search and Input

**User Story:** As a user, I want to search for routes using text or voice input, so that I can quickly find transport options in my preferred interaction mode.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL accept destination input via text entry
2. WHERE voice-first mode is enabled, THE SafeRoute_System SHALL accept destination input via voice recognition
3. WHEN a user speaks a destination, THE SafeRoute_System SHALL convert speech to text within 2 seconds
4. WHEN a user enters an ambiguous location name, THE SafeRoute_System SHALL present up to 5 matching location suggestions
5. WHEN a user selects a destination, THE SafeRoute_System SHALL calculate routes from current location to the destination

### Requirement 5: Multi-Modal Route Generation

**User Story:** As a user, I want to see multiple public transport route options, so that I can choose the route that best balances convenience and safety.

#### Acceptance Criteria

1. WHEN a user requests routes, THE SafeRoute_System SHALL generate at least 3 alternative Multi_Modal_Routes when available
2. THE SafeRoute_System SHALL include bus routes with stop-to-stop segments
3. THE SafeRoute_System SHALL include metro and train routes with station-to-station segments
4. THE SafeRoute_System SHALL include walking segments connecting transport modes
5. WHEN generating routes, THE SafeRoute_System SHALL calculate estimated travel time for each route
6. WHEN generating routes, THE SafeRoute_System SHALL respect user transport preferences from their profile

### Requirement 6: Safety Risk Scoring

**User Story:** As a user, I want each route segment to have a safety score, so that I can make informed decisions about which route to take.

#### Acceptance Criteria

1. WHEN a route is generated, THE Risk_Scoring_Engine SHALL calculate a Safety_Score for each Route_Segment
2. THE Risk_Scoring_Engine SHALL incorporate reported incident frequency in the Safety_Score calculation
3. THE Risk_Scoring_Engine SHALL incorporate lighting conditions in the Safety_Score calculation
4. THE Risk_Scoring_Engine SHALL incorporate crowd density patterns in the Safety_Score calculation
5. THE Risk_Scoring_Engine SHALL incorporate proximity to Help_Points in the Safety_Score calculation
6. THE Risk_Scoring_Engine SHALL adjust Safety_Scores based on time of day context
7. THE Risk_Scoring_Engine SHALL adjust Safety_Scores based on day type context
8. WHEN a Route_Segment has a Safety_Score of 70-100, THE SafeRoute_System SHALL classify it as "Safer"
9. WHEN a Route_Segment has a Safety_Score of 40-69, THE SafeRoute_System SHALL classify it as "Moderate"
10. WHEN a Route_Segment has a Safety_Score of 0-39, THE SafeRoute_System SHALL classify it as "Less Safe"

### Requirement 7: Safety Visualization

**User Story:** As a user, I want to see safety information displayed clearly on a map, so that I can quickly understand which parts of my route are safer.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL display route segments on a map interface
2. WHEN displaying a Route_Segment classified as "Safer", THE SafeRoute_System SHALL render it in green color
3. WHEN displaying a Route_Segment classified as "Moderate", THE SafeRoute_System SHALL render it in yellow color
4. WHEN displaying a Route_Segment classified as "Less Safe", THE SafeRoute_System SHALL render it in red color
5. THE SafeRoute_System SHALL display safety icons at bus stops indicating their Safety_Rating
6. THE SafeRoute_System SHALL display safety icons at metro and train stations indicating their Safety_Rating
7. THE SafeRoute_System SHALL display Help_Point locations on the map with distinctive markers
8. WHEN a user taps a Route_Segment, THE SafeRoute_System SHALL display detailed safety information for that segment

### Requirement 8: Voice Assistance and Guidance

**User Story:** As a user, I want voice guidance during my journey, so that I can navigate safely without constantly looking at my phone screen.

#### Acceptance Criteria

1. WHERE voice-first mode is enabled, THE SafeRoute_System SHALL provide spoken route guidance
2. WHEN a route begins, THE SafeRoute_System SHALL announce the first navigation instruction via voice
3. THE SafeRoute_System SHALL provide voice guidance using calm, non-alarming language
4. WHEN approaching a transport stop, THE SafeRoute_System SHALL announce the stop name and safety context via voice
5. THE SafeRoute_System SHALL generate voice guidance within 500 milliseconds of a navigation event
6. WHERE silent mode is enabled, THE SafeRoute_System SHALL provide text-only guidance without voice output

### Requirement 9: Live Trip Monitoring and Safety Mode

**User Story:** As a user, I want to activate Safety Mode during my journey, so that the app monitors my location and can alert my trusted contacts if something goes wrong.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL allow users to activate Safety_Mode before or during a journey
2. WHEN Safety_Mode is activated, THE SafeRoute_System SHALL track user location in real-time with updates every 30 seconds
3. WHEN Safety_Mode is activated, THE SafeRoute_System SHALL display a prominent SOS_Button on the screen
4. WHEN the user's location indicates a Route_Deviation, THE SafeRoute_System SHALL send an alert notification to the user
5. WHEN Safety_Mode is activated, THE SafeRoute_System SHALL allow users to share live location with up to 5 Trusted_Contacts
6. WHEN a Trusted_Contact receives a location share, THE SafeRoute_System SHALL provide them with a live map view of the user's location
7. WHEN Safety_Mode is active for more than 3 hours, THE SafeRoute_System SHALL prompt the user to confirm continued monitoring

### Requirement 10: Emergency SOS Functionality

**User Story:** As a user in distress, I want to trigger an emergency alert with one tap, so that my trusted contacts and authorities are immediately notified of my situation.

#### Acceptance Criteria

1. WHEN a user taps the SOS_Button, THE SafeRoute_System SHALL send immediate notifications to all designated Trusted_Contacts
2. WHEN a user taps the SOS_Button, THE SafeRoute_System SHALL include current GPS coordinates in the notification
3. WHEN a user taps the SOS_Button, THE SafeRoute_System SHALL include a timestamp in the notification
4. WHEN a user taps the SOS_Button, THE SafeRoute_System SHALL provide a direct link to the user's live location
5. THE SafeRoute_System SHALL process SOS_Button activation within 1 second
6. WHEN the SOS_Button is activated, THE SafeRoute_System SHALL continue tracking location until manually deactivated by the user

### Requirement 11: Crowdsourced Safety Feedback

**User Story:** As a user who has completed a journey, I want to rate the safety of my route and report unsafe conditions, so that I can help improve safety information for other users.

#### Acceptance Criteria

1. WHEN a user completes a journey, THE SafeRoute_System SHALL prompt the user to rate the route safety
2. THE SafeRoute_System SHALL accept safety ratings on a scale of 1-5 stars
3. THE SafeRoute_System SHALL allow users to submit Incident_Reports at any time
4. WHEN submitting an Incident_Report, THE SafeRoute_System SHALL allow users to specify the location
5. WHEN submitting an Incident_Report, THE SafeRoute_System SHALL allow users to select incident type from predefined categories
6. THE SafeRoute_System SHALL submit all Incident_Reports anonymously without revealing user identity
7. WHEN an Incident_Report is submitted, THE SafeRoute_System SHALL incorporate the data into future Safety_Score calculations within 24 hours

### Requirement 12: Trusted Contact Management

**User Story:** As a user, I want to designate trusted contacts who can receive my location during Safety Mode, so that people I trust can monitor my journey when needed.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL allow users to add up to 5 Trusted_Contacts
2. WHEN adding a Trusted_Contact, THE SafeRoute_System SHALL require a phone number or email address
3. WHEN a Trusted_Contact is added, THE SafeRoute_System SHALL send a notification to that contact requesting consent
4. THE SafeRoute_System SHALL only activate location sharing with Trusted_Contacts who have provided consent
5. THE SafeRoute_System SHALL allow users to remove Trusted_Contacts at any time

### Requirement 13: Admin Dashboard and Safety Monitoring

**User Story:** As a city administrator, I want to view safety heatmaps and incident data, so that I can identify problem areas and allocate resources effectively.

#### Acceptance Criteria

1. THE Admin_Dashboard SHALL display a heatmap visualization of safety scores across the city
2. THE Admin_Dashboard SHALL display aggregated Incident_Report data by location
3. THE Admin_Dashboard SHALL allow administrators to filter data by time period
4. THE Admin_Dashboard SHALL allow administrators to filter data by incident type
5. THE Admin_Dashboard SHALL display the locations of all registered Help_Points
6. THE Admin_Dashboard SHALL allow administrators to add new Help_Point locations
7. THE Admin_Dashboard SHALL allow administrators to update existing Help_Point information
8. THE Admin_Dashboard SHALL display incident trends over time with graphical visualizations

### Requirement 14: Data Privacy and Security

**User Story:** As a user, I want my personal data and location history to be protected, so that I can use the app without privacy concerns.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL encrypt all user location data during transmission using TLS 1.3 or higher
2. THE SafeRoute_System SHALL encrypt all user location data at rest using AES-256 encryption
3. THE SafeRoute_System SHALL encrypt all Trusted_Contact information at rest
4. THE SafeRoute_System SHALL not display individual user routes on public interfaces or the Admin_Dashboard
5. WHEN storing Incident_Reports, THE SafeRoute_System SHALL remove all personally identifiable information
6. THE SafeRoute_System SHALL allow users to delete their account and all associated data
7. WHEN a user deletes their account, THE SafeRoute_System SHALL permanently remove all personal data within 30 days

### Requirement 15: Performance and Responsiveness

**User Story:** As a user, I want the app to respond quickly to my requests, so that I can make timely decisions about my travel.

#### Acceptance Criteria

1. WHEN a user requests routes, THE SafeRoute_System SHALL return route suggestions within 3 seconds
2. WHEN a user activates voice input, THE SafeRoute_System SHALL begin listening within 500 milliseconds
3. WHEN the SOS_Button is pressed, THE SafeRoute_System SHALL send notifications within 1 second
4. WHEN Safety_Mode is active, THE SafeRoute_System SHALL update location tracking every 30 seconds
5. WHEN displaying a map, THE SafeRoute_System SHALL render all route segments within 2 seconds

### Requirement 16: Offline Functionality and Reliability

**User Story:** As a user in an area with poor connectivity, I want the app to continue functioning with cached data, so that I can still access basic navigation even without internet.

#### Acceptance Criteria

1. WHEN internet connectivity is unavailable, THE SafeRoute_System SHALL provide route navigation using cached map data
2. WHEN internet connectivity is unavailable, THE SafeRoute_System SHALL display previously calculated Safety_Scores
3. WHEN Safety_Score data is unavailable for a route, THE SafeRoute_System SHALL generate routes using standard routing algorithms
4. WHEN Safety_Score data is unavailable, THE SafeRoute_System SHALL display a notice indicating that safety information is not current
5. WHEN internet connectivity is restored, THE SafeRoute_System SHALL synchronize cached data with the server within 60 seconds

### Requirement 17: Accessibility Features

**User Story:** As a user with visual impairments, I want the app to work with screen readers and provide clear audio feedback, so that I can navigate safely independently.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL provide screen reader compatibility for all interactive elements
2. THE SafeRoute_System SHALL provide text descriptions for all map icons and visual elements
3. THE SafeRoute_System SHALL ensure the SOS_Button has a minimum touch target size of 44x44 pixels
4. THE SafeRoute_System SHALL support high contrast color modes for improved visibility
5. THE SafeRoute_System SHALL provide haptic feedback when the SOS_Button is activated
6. THE SafeRoute_System SHALL allow font size adjustment from 100% to 200% of default size

### Requirement 18: Multi-City Scalability

**User Story:** As a product manager, I want the system architecture to support multiple cities, so that we can expand SafeRoute to new locations without major redesign.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL store city-specific data in separate logical partitions
2. THE SafeRoute_System SHALL allow administrators to configure new cities through the Admin_Dashboard
3. WHEN a user's location is in an unsupported city, THE SafeRoute_System SHALL display a message indicating service unavailability
4. THE SafeRoute_System SHALL allow users to manually select a city for route planning
5. THE Risk_Scoring_Engine SHALL calculate Safety_Scores independently for each city using city-specific data sources

### Requirement 19: LLM-Powered Safety Explanations

**User Story:** As a user, I want to understand why a route is rated as safer or less safe, so that I can make informed decisions based on clear reasoning.

#### Acceptance Criteria

1. WHEN a user requests explanation for a Safety_Score, THE SafeRoute_System SHALL generate a natural language explanation
2. THE SafeRoute_System SHALL generate explanations using calm, non-alarming language
3. THE SafeRoute_System SHALL include specific factors contributing to the Safety_Score in the explanation
4. THE SafeRoute_System SHALL generate explanations within 2 seconds of user request
5. WHERE voice-first mode is enabled, THE SafeRoute_System SHALL provide spoken explanations

### Requirement 20: Transport Schedule Integration

**User Story:** As a user, I want to see real-time transport schedules integrated with safety information, so that I can choose both safe and timely routes.

#### Acceptance Criteria

1. THE SafeRoute_System SHALL retrieve real-time bus schedules from city transport APIs
2. THE SafeRoute_System SHALL retrieve real-time metro and train schedules from city transport APIs
3. WHEN displaying a route, THE SafeRoute_System SHALL show expected arrival times at each stop
4. WHEN a transport service is delayed, THE SafeRoute_System SHALL update route timing within 60 seconds
5. WHEN a transport service is cancelled, THE SafeRoute_System SHALL remove that option from route suggestions
