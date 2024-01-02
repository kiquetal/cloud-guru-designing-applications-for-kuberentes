### Processes

A twelve-factor app runs as 1 or more processes. These processes are utimately
managed by the host OS.

Stateless: They do not store persistent data in-memory and can be restarted or replaced freely.

Share-Nothing: do no directly share memory or storage. Additional processes
can be added to scale the app horizontally.

