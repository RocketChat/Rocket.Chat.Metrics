## Totals

This section shows total values for various statistics, visualized as singlestat.

### Users Online / Users Away

Users currently online/away.

### DDP Users 

Connected DDP users including mobile/alternative clients, bots, APIs, etc.

### Users / Rooms / Messages

Total amount of users / rooms / messages.

### Users Diff / Rooms Diff / Messages Diff

Difference (based on the selected time in Grafana's time picker on the top right) of users / rooms / messages.

## Metrics

Information about the metrics itself, visualized in graphs.

### Metrics Requests

Shows how many requests agains Rocket.Chat's Prometheus exporter endpoint (`http://rocketchat/metrics`) have been made so far.

### Metrics Size

Shows the total size (in kB) of Rocket.Chat's Prometheus exporter endpoint HTTP response.

## Node JS

Information about the NodeJS environment, visualized in graphs.

### Active Handles

Shows active handles (can be opened files, database connections or network requests) per instance.

### Active Requests

Shows requests handled per instance.

### Event loop lag

Displays potential lag in event loop per instance.

### Heap Used

Shows total JavaScript heap memory in use.

### Per pod heap

Shows JavaScript heap memory per instance.

### GC - Seconds Running

Show runtime of each garbage collector tasks. (Needs to be enabled first in Rocket.Chat's admin UI under "Logs" → "Prometheus" → "Collect NodeJS GC")

### GC - Bytes Freed

Show how much space garbage collector could free per instance. (Needs to be enabled first in Rocket.Chat's admin UI under "Logs" → "Prometheus" → "Collect NodeJS GC")

## DDP Rate Limiter

Shows information about the builtin DDP rate limiter. Will only show data in case the rate limiter is actually active and collecting limit violations, visualized in graphs.

### Limit by type

Shows violation by type.

### Limit by method

Shows violation by application methods.

### Limit by userId

Shows violation by userId.

### Limit by connectionId

Shows violation by connectionId.

## RocketChat Data

This section contains information about Rocket.Chat itself, visualized in graphs.

### Messages Sent (30m)

Shows messages sent in the past 30 minutes, per instance.

### Messages Sent Total

Displays total messages sent.

### Users Presence

Shows online and away users.

### Users & Sessions

Shows all established (alternative clients, API, bots)) as well as authenticated sessions.

### Web Socket Connections per pod

Shows established web socket connections, per instance.

### Apps Installed / Disabled / Failed

Displays installed / disabled and failed apps, per instance.

### Oplog

Shows specific oplog events for each collection.

### Oplog By instance

Shows amount of oplog events, per instance.

### Oplog Queue

Shows oplog events in queue (in case there are any).

### Push Queue

Displays amount of push notifications in queue.

### Total Users

Shows the total amount of users.

### Total of Rooms

Shows the total amount of created rooms.

### Notifications / Minute

Displays the notification per minute throughput.

## Methods

Shows the timing of exectued functions, visualized in graphs. Useful to troubleshoot application performance issues.

## Subscriptions

Shows the timing of consumed subscriptions, visualized in graphs. Useful to troubleshoot application performance issues.

## Callbacks & Hooks

Shows the timing of triggered callbacks and hooks, visualized in graphs. Useful to troubleshoot application performance issues.

## REST API

Shows the timing of triggered REST API endpoints, visualized in graphs. Useful to troubleshoot application performance issues.
