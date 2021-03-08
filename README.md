This is an example of a simple android application which uses different ways to specify build's `versionCode`.

In particular, the version code is the incremented latest build number fetched from Google Play Developer API throught [`google-play`](https://github.com/codemagic-ci-cd/cli-tools/tree/master/docs/google-play#google-play) Codemagic CLI Tool.

### Pass build number with an environment variable

The latest Google Play build number is obtained in [`codemagic.yaml`](./codemagic.yaml) and is exported to `LATEST_GOOGLE_PLAY_BUILD_NUMBER` (arbitrary name)

```bash
export LATEST_GOOGLE_PLAY_BUILD_NUMBER=$(google-play get-latest-build-number --package-name 'io.codemagic.androidhelloworldapp')
```

In [`build.gradle`](./app/build.gradle), the variable is read and is used in android config to create a version code and version name

```java
android {
    ...
    // try to get build number from `LATEST_GOOGLE_PLAY_BUILD_NUMBER` env var. If not specified, then from `BUILD_NUMBER` env var, else 0
    def latestGooglePlayBuildNumber = Integer.valueOf(System.env.LATEST_GOOGLE_PLAY_BUILD_NUMBER ?: System.env.BUILD_NUMBER ?: 0)
    defaultConfig {
        ...
        versionCode latestGooglePlayBuildNumber + 1
        versionName "1.0." + String.valueOf(latestGooglePlayBuildNumber + 1)
```

### Check other methods at:

-  [`master`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/master) branch

    Pass the desired version code with gradlew argument properties

- [`autoversioning_through_file`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_file) branch

    Read `versionCode` in `build.gradle` from a created `app/version.properties` file

- [`autoversioning_through_executable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_executable) branch

    Call the `google-play` tool directly from the `build.gradle` to get the build version Code
