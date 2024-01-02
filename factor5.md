### Factor V: Build, Release Run
A twelve-factor App has separated build,
release and run stages. These stages govern how code is deployed.

Portable build, configured release

A build produces an executable that can run in any environment.
The release stage provides environment-specific configuration.
The run stage is low-complexity. The app can be run,started or restarted
without developer involvement.

#### Build

Docker helps design and build container images.


docker build -t myapp:0.0.1 --target server .


#### Deployments

Can help wih the release stage, using the deployments to manage a desired state
for your application and provide it with configuration through environment variables.

