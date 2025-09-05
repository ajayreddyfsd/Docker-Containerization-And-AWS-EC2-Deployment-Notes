### 🐳 The Docker Process Diagram

Here is a simple diagram that visualizes the core Docker workflow you described, from code to a running container.

```
+-----------------+
| Your Application|
| Code            |
+-----------------+
        |
        v
+-----------------+
|    Dockerfile   | <---- You write the instructions here
| (Instructions)  |
+-----------------+
        |
        v
+-----------------+
|     Build       | <---- "docker build" command reads the Dockerfile and create docker-image
|   (creates)     |
+-----------------+
        |
        v
+-----------------+
|      Image      |
|  (a blueprint)  |
+-----------------+
        |
        v
+-----------------+
|      Run        | <---- "docker run" command creates docker-container from the image and runs it
|   (creates &)   |
|   (starts)      |
+-----------------+
        |
        v
+-----------------+
|    Container    |
|  (a running     |
|  instance)      |
+-----------------+
```
