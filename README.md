# Event Registration API (Lab 8)

Spring Boot REST API for events with CRUD operations. Data is stored in memory
(a Java list), so all changes disappear when the application restarts.

## Run

Requirements: JDK 17 or newer, Gradle wrapper (included).

```bash
./gradlew bootRun        # Windows: gradlew.bat bootRun
```

The app starts on http://localhost:8080.

## Endpoints

| Method | URL | Description | Success | Errors |
| --- | --- | --- | --- | --- |
| GET | `/api/events` | All events. Optional `?title=quiz` (case-insensitive contains) and `?location=room` | 200 | - |
| GET | `/api/events/{id}` | One event | 200 | 404 |
| POST | `/api/events` | Create event (ID assigned by server) | 201 | 400 |
| PUT | `/api/events/{id}` | Replace title and location, keep ID | 200 | 400, 404 |
| DELETE | `/api/events/{id}` | Delete event, no body | 204 | 404 |

A missing or blank `title` or `location` returns 400 and the list is not changed.

## Example JSON body (POST / PUT)

```json
{
  "title": "Web Design Workshop",
  "location": "Room 101"
}
```

## curl examples

```bash
curl http://localhost:8080/api/events
curl "http://localhost:8080/api/events?title=quiz"
curl -i -X POST http://localhost:8080/api/events -H "Content-Type: application/json" -d '{"title":"Hackathon","location":"Lab 3"}'
curl -i -X PUT http://localhost:8080/api/events/4 -H "Content-Type: application/json" -d '{"title":"Hackathon 2026","location":"Lab 5"}'
curl -i -X DELETE http://localhost:8080/api/events/4
```

## Structure

- `EventInfo` - model (id, title, location)
- `EventService` - in-memory list, ID counter, search/create/update/delete
- `RegistrationController` - HTTP layer; the service is injected via constructor
