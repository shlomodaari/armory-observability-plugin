# Armory Observability Plugin

Spinnaker plugin for configuring and customizing observability features such as
- Customizing and tweaking the Micrometer registry.
- Enabling direct transmision of metrics to promethous and skipping the [Spinnaker Monitoring Daemon](https://github.com/spinnaker/spinnaker-monitoring/tree/master/spinnaker-monitoring-daemon)

## Potential Future Additions
- Enable distrubuted tracing, Slueth?
- Customize logging to filter noise or have custom appender for shipping important logs to log aggregator
- Customize Error handling? Can we enable something like [Backstopper](https://github.com/Nike-Inc/backstopper) in a plugin? I have no idea ¯\_(ツ)_/¯.

## Development

1) Run `./gradlew releaseBundle`
2) Put the `/build/distributions/pf4jPluginWithoutExtensionPoint-X.zip` in the [configured plugins location for your service](https://pf4j.org/doc/packaging.html).
3) [Configure the plugin Spinnaker service.](#plugin-configuration)

To debug the plugin inside a Spinnaker service (like Orca) using IntelliJ Idea follow these steps:

1) Run `./gradlew releaseBundle` in the plugin project.
2) Copy the generated `.plugin-ref` file under `build` in the plugin project submodule for the service to the `plugins` directory under root in the Spinnaker service that will use the plugin .
3) Link the plugin project to the service project in IntelliJ (from the service project use the `+` button in the Gradle tab and select the plugin build.gradle).
4) Configure the Spinnaker service the same way specified above.
5) Create a new IntelliJ run configuration for the service that has the VM option `-Dpf4j.mode=development` and does a `Build Project` before launch.
6) Debug away...

## Plugin Configuration

```yaml
spinnaker:
  extensibility:
    plugins:
      armory.obervabilityPlugin:
        enabled: true
        config:
          # Human Readable Customer name for dashboarding.
          customerName: armory
          # Human Readable Customer Environment name for dashboarding.
          customerEnvName: production
          # Halyard generated UUID for non-managed and non-sass customers
          customerEnvId: e0fb0422-aa8e-11ea-bb37-0242ac130002
```
