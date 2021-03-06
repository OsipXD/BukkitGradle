## [Unreleased]

## [0.9.2] - 2020-01-25

### Fixed
- Fixed task `copyPlugins` when shadow plugin is enabled

## [0.9.1] - 2020-01-16

### Added
- `codemc()` repository extension

### Changed
- **BREAKING CHANGE**: `bukkit` dependency extension renamed to `bukkitApi`.
  This name is more consistent, also it avoids conflict with `BukkitExtension` extension name.

## [0.9.0] - 2020-01-08

### Reworked tasks hierarchy

All plugin's tasks reworked to use actual Gradle APIs:
- [Task Configuration Avoidance][tca].
  Plugin's tasks will be created and configured only when it needed
- [Up-to-date checks][uptodate].
  Tasks will not re-run if input data not changed
- Optimized work in offline mode.

### Property syntax to configure meta fields
Use `.set` instead of `=`:
```diff
bukkit {
    meta {
-        desctiption = "My plugin's description"
+        description.set("My plugin's description")
    }
}
```

### Improved Bukkit version management

Field `bukkit.version` is deprecated now, you should use `bukkit.apiVersion` instead.
Also, you can specify a version for dev server different from `apiVersion`:
```kotlin
bukkit {
    apiVersion = "1.16.4"
    server {
        version = "1.15.2" // Want to test plugin on older minecraft version 
    }
}
```
If `bukkit.server.version` is not specified, will be used `bukkit.apiVersion` for server.

Now, BukkitGradle adds `api-version` field to `plugin.yml`.
It will be parsed from `bukkit.apiVersion` but you can override it with `bukkit.meta.apiVersion` if need:
```kotlin
bukkit {
    apiVersion = "1.16.4" // Inferred api-version is 1.16
    
    meta {
        apiVersion.set("1.13") // But here you can override it
    }
}
```

### Smarter plugin.yml generation

BukkitGradle will not "eat" your existing plugin.yml file.
Its content will be used if you've not configured `bukkit.meta` in BukkitGradle.

If you don't want plugin.yml generation at all, you can disable it:
```kotlin
bukkit {
    disableMetaGeneration()
}
```

### Re-written in Kotlin
The plugin has been converted to Kotlin to make support easier.
The plugin still can be configured with Groovy DSL but
now it is friendly to Kotlin DSL.

### Removed
- Removed task `:rebuildServerCore`, use `:buildServerCore --rerun-tasks` instead
- Removed extension `DependencyHandler.craftbukkit()`, use `DependencyHandler.spigot()` instead
- Removed automatic `mavenLocal()` apply, you should apply it manually if you need it
- Removed repository extension `RepositoryHandler.vault()`, use `RepositoryHandler.jitpack()` instead and read [VaultAPI README][vault]

### Changed
- `bukkit.run` renamed to `bukkit.server`. The old name is deprecated
- Add `nogui` argument by default to `bukkitArgs`
- Type of properties `server.jvmArgs` and `server.bukkitArgs` changed from `String` to `List<String>`.
  It makes it easier to add arguments without overriding defaults
- Default main class pattern changed from `<groupId>.<lowercased name>.<name>` to `<groupId>.<name>`

### Housekeeping
- Default bukkit version now is 1.16.4
- Update Gradle to 6.7.1

[tca]: https://docs.gradle.org/current/userguide/task_configuration_avoidance.html
[uptodate]: https://docs.gradle.org/current/userguide/more_about_tasks.html#sec:up_to_date_checks
[vault]: https://github.com/MilkBowl/VaultAPI

[unreleased]: https://github.com/EndlessCodeGroup/BukkitGradle/compare/0.9.2...develop
[0.9.2]: https://github.com/EndlessCodeGroup/BukkitGradle/compare/0.9.1...0.9.2
[0.9.1]: https://github.com/EndlessCodeGroup/BukkitGradle/compare/0.9.0...0.9.1
[0.9.0]: https://github.com/EndlessCodeGroup/BukkitGradle/compare/0.8.2...0.9.0
