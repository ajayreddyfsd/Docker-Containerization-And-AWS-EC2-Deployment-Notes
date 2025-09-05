
### 📝 Process Diagram: Containerizing a Full-Stack Application with Docker

Here is a simple, text-based diagram illustrating the steps you described for containerizing an application.

```
+-------------------+      +-----------------+      +---------------------+
|    Project Root   |      |   Client (src)  |      |   Server (src)      |
|  (Dockerfile)     |      |  (package.json) |      |  (package.json)     |
+-------------------+      +-----------------+      +---------------------+
         |                          |                           |
         +--------------------------+---------------------------+
         |                          |                           |
         v                          v                           v
+-------------------+      +-----------------+      +---------------------+
| Adjust Frontend   |      | Create "build"  |      | Create "build"      |
| Backend URL       |      | folder via      |      | folder via          |
| (No localhost)    |      | "npm run build" |      | "npm run build"     |
+-------------------+      +-----------------+      +---------------------+
         |
         v
+-------------------+
|  Create Dockerfile|
| (in Project Root) |
+-------------------+
         |
         v
+-------------------+
|   Build Image     | <---- "docker build -t <name> ."
| (Docker Desktop)  |
+-------------------+
         |
         v
+-------------------+
|   Run Container   | <---- "docker run -p <host:container>"
| (Accessible via   |
|  localhost:<port>)|
+-------------------+
```