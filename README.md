## GeoFilm: Film Location Explorer

GeoFilm is a web application developed with [Spring Boot](https://spring.io/projects/spring-boot), designed to help users discover and explore real-world locations where iconic scenes from movies and television series were filmed. The project showcases advanced backend technologies, external data integrations, and modern development practices in a layered, modular architecture.

### Project Purpose

This project began as a demonstration during an intensive two-week internship, aiming to answer a common question for cinema fans and travelers: *Where was my favorite movie scene filmed?* GeoFilm lets users search for films and TV shows, identify filming locations, and save favorite places for future reference. The result is a robust tool with valuable features for cinephiles, travelers, and developers interested in location-based data.

---

## Technologies & Architecture

- **Framework:** Spring Boot 3.5.0 (Java 17)
- **Database:** MySQL using Hibernate/JPA for object-relational mapping
- **Web Scraping & HTML Parsing:** Selenium WebDriver (Firefox), Jsoup
- **External Integrations:**
  - *OMDb API*: Retrieves detailed film information
  - *IMDb Web Scraping*: Automated extraction of filming locations with Selenium
  - *DistanceMatrix AI*: Geocoding service for converting addresses to geographical coordinates (latitude/longitude)
- **Security:** Spring Security and BCrypt password encryption
- **Layered Architecture:** Clear separation of concerns among business logic, data access, entities, controllers, and data transfer objects

---

## Core Features

### 1. Film & TV Show Search

Users can search movies and TV shows by name or IMDb ID. The backend queries the public [OMDb API](https://www.omdbapi.com/) to obtain metadata such as title, poster, synopsis, main cast, release year, etc.

### 2. Automatic Location Discovery

GeoFilm employs Selenium WebDriver and web scraping to visit IMDb pages and extract filming locations. Locations may include real or fictional addresses and are encapsulated in `MediaLocation` objects.

#### Example Flow:
- User searches for "Inception."
- The backend contacts IMDb and identifies shooting locations like "Paris, France" or "Los Angeles, California, USA."
- These locations are stored and returned with the movie information.

### 3. Geocoding (Getting Coordinates)

After extracting locations, the application sends them to DistanceMatrix AI to obtain their geocoordinates:
- Each address is queried, and latitude/longitude are saved.
- If a location cannot be resolved, null is returned and an error is logged.

This enables visualization on maps or as points of geographic interest.

### 4. User & Favorites Management

Users can register, log in, and save favorite filming locations to their profiles:
- Many-to-many relationships link users and locations.
- Favorites can be queried, deleted, or added via dedicated API endpoints.

Authentication is secured using Spring Security and bcrypt password hashing.

### 5. RESTful API

GeoFilm provides a comprehensive REST API, ready for integration with modern frontends (Angular, React, etc). Example endpoints:
- **POST /api/v1/media/search/name:** Search for films by name
- **POST /api/v1/media/search/id:** Search by IMDb ID
- **POST /api/v1/media/search/predict/name:** Get keyword predictions
- **GET/POST /api/v1/profile/favlocs/list:** Retrieve the authenticated user’s favorite locations

---

## Detailed Architecture

- **JPA Entities:** Represent users and locations, supporting complex relationships and efficient SQL management
- **Media Objects:** Encapsulate movies, episodes, or programs including their discovered locations and cast data
- **Service Layer:** Handles business logic (search, authentication, location management)
- **REST Controllers:** Accept external requests, validate input, and return structured responses
- **Spring Data JPA Repositories:** Abstract database operations, minimizing manual SQL coding

Each component interacts in a layered manner, promoting maintainability and scalability.

---

## External Integrations & Data Flow

1. **OMDb API:**
   - Fetches movie ID, title, poster, and synopsis
   - JSON responses are parsed into internal Media objects

2. **IMDb Web Scraping (Selenium + Jsoup):**
   - Navigates to IMDb filming location pages
   - Dynamically extracts addresses using automation and HTML parsing

3. **DistanceMatrix AI (Geocoding):**
   - Sends addresses and receives geocoordinate data via HTTP requests
   - Coordinates are stored in MediaLocation objects

---

## Example Usage

1. User searches for a movie via the frontend.
2. The backend queries OMDb for relevant details.
3. GeoFilm scrapes IMDb for all filming locations.
4. Each address is geocoded and linked to the movie.
5. The user can view, save, or delete locations as favorites.
6. All relevant data (users, favorites, locations) is managed via secure, well-documented API endpoints.

---

## Error Handling & Security

- All relevant exceptions are caught and managed, ensuring the user receives informative responses (user not found, invalid credentials, external integration errors, invalid data, etc).
- Passwords are securely stored and never exposed in plaintext.
- The system replies with user-friendly messages for any errors or external API problems.

---

## Contributors

GeoFilm was collaboratively developed by [monstahcode](https://github.com/monstahcode), [IvanR05](https://github.com/IvanR05),  [Grifo999](https://github.com/Grifo999) , and [Croack28](https://github.com/Croack28) over two weeks. The team successfully integrated web scraping, advanced data management, secure authentication, and a maintainable API suitable for future growth.

---

## Setup & Getting Started

**Prerequisites:**
- Java 17+
- Maven 3.6+
- Running MySQL database
- Installed Firefox browser and GeckoDriver

**Setup Steps:**
1. Clone the repository
2. Configure your MySQL connection in `application.properties`
3. Install GeckoDriver and ensure Firefox is operational
4. Run:
   ```bash
   mvn spring-boot:run
   ```

---

## Extra Notes

GeoFilm exemplifies how powerful, extensible software can be created rapidly through teamwork and modern technology. Utilizing web scraping, API integration, and secure development practices in Java, it serves as an ideal reference for developers interested in location-based entertainment applications.

---

Questions, feedback, or contributions? Feel free to review the code, open issues, or join future development!
