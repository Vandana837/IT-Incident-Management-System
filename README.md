# IT Incident Management System

A beginner-friendly Java 17 console application for recording, assigning, tracking, and reporting IT incidents. It demonstrates object-oriented design, `java.time`, CSV persistence, validation, and JUnit 5 testing without a database or network service.

## Problem statement

Support teams need a simple way to track incidents from creation through resolution, identify urgent or overdue work, and measure workload. This project models that workflow locally for learning and demonstrations.

## Features

- Create incidents with generated IDs, categories, priorities, statuses, and configurable deadlines.
- View, search, assign/reassign, update, filter by status/priority/category, and delete incidents.
- Detect overdue unresolved incidents using the current date and time.
- Generate status, priority, assignment, overdue, and resolution summaries.
- Persist data in `data/incidents.csv` with quoted CSV fields.
- Recover gracefully from invalid console input and report malformed storage records.

## Technology and prerequisites

- Java 17 or later
- Apache Maven 3.8+
- Any IDE or terminal (VS Code works well)
- No runtime internet connection, API key, or external service is required.

## Project structure

```text
src/main/java/com/vandana/incidentmanagement/
  Main.java
  model/Incident.java, Category.java, Priority.java, IncidentStatus.java
  service/IncidentManager.java, ReportService.java
  storage/FileStorage.java
src/test/java/com/vandana/incidentmanagement/IncidentManagerTest.java
pom.xml
README.md
```

## Build and run

From the project directory:

```bash
mvn clean test
mvn package
mvn exec:java
```

The application creates `data/incidents.csv` automatically. The file is intentionally ignored by Git because it contains local runtime data. Descriptions and titles are quoted and escaped so commas, quotation marks, and line breaks are preserved.

## Sample menu

```text
=== IT Incident Management System ===
1. Create Incident
2. View All Incidents
...
12. Exit
Choose an option:
```

Use dates in `yyyy-MM-dd HH:mm` format. A typical workflow is:

1. Choose **1**, enter “VPN unavailable”, category Network, priority High, status Open, and a due time.
2. Choose **4**, enter the generated ID (for example `INC-0001`) and an engineer name.
3. Choose **5**, change the status to In Progress and later Resolved.
4. Choose **9** to see unresolved incidents past their due time, or **10** for a summary.
5. Choose **11** and type `YES` to delete an incident.

## Testing

`mvn clean test` runs tests covering creation/search, invalid data, updates, assignment, filtering, controlled-time overdue detection, report calculations including an empty list, and CSV save/reload.

## Limitations and future enhancements

This educational version has one local CSV file, one console user, and no authentication or concurrent editing. Future versions could add configurable SLA rules, CSV backup/restore, pagination, and a database or web interface.

## Interview questions to practise

1. **Why use enums?** They restrict category, priority, and status values to a known set.
2. **Why use `LocalDateTime`?** It represents date and time clearly and avoids legacy date APIs.
3. **Why inject `Clock`?** Production code uses the real clock, while tests can use a fixed clock.
4. **How is data persisted?** `FileStorage` writes one incident per quoted CSV record and reloads it at startup.
5. **How are duplicate IDs avoided?** IDs are generated from the highest existing numeric incident ID.
6. **What does encapsulation mean here?** Fields are private and changes happen through controlled methods.
7. **How is overdue status decided?** An unresolved incident is overdue when its due time is before the comparison time.
8. **How is division by zero avoided?** An empty report returns a zero resolution percentage.
9. **Why separate services from `Main`?** Business logic can be tested without console input/output.
10. **What are checked exceptions used for?** File I/O failures are surfaced so users know persistence did not complete.
