This is an example of a simple android application which uses different ways to specify build's `versionCode`.

In particular, the version code is the incremented latest build number fetched from Google Play Developer API throught [`google-play`](https://github.com/codemagic-ci-cd/cli-tools/tree/master/docs/google-play#google-play) Codemagic CLI Tool.

### Call the [`google-play`](https://github.com/codemagic-ci-cd/cli-tools/tree/master/docs/google-play#google-play) tool directly from the `build.gradle` to get the build number

No changes required to [`codemagic.yaml`](./codemagic.yaml).

In [`build.gradle`](./app/build.gradle)'s android config, a shell command is executed to get the latest google play build number. It is incremented and used to specify the `versionCode` and `versionName`

```java
android {
    def packageName = "io.codemagic.androidhelloworldapp"
    def latestGooglePlayBuildNumber = "google-play get-latest-build-number --package-name $packageName".execute().text
    def updatedBuildNumber = latestGooglePlayBuildNumber.toInteger() + 1 // will fail the build if latestGooglePlayBuildNumber is not found
    println("Building with version code $updatedBuildNumber")

    ...
    defaultConfig {
        ...
        versionCode updatedBuildNumber
        versionName "1.0" + updatedBuildNumber.toString()
    }
```

### Check other methods at:

-  [`master`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/master) branch

    Pass the desired version code with gradlew argument properties

- [`autoversioning_through_file`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_file) branch

    Read `versionCode` in `build.gradle` from a created `app/version.properties` file

- [`autoversioning_through_environment_variable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_environment_variable) branch

    Read a created environment variable with build number in `build.gradle`
