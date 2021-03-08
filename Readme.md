This is an example of a simple android application which uses different ways to specify build's `versionCode`.

In particular, the version code is the incremented latest build number fetched from Google Play Developer API throught [`google-play`](https://github.com/codemagic-ci-cd/cli-tools/tree/master/docs/google-play#google-play) Codemagic CLI Tool.

### Pass build number with gradlew argument properties

The latest Google Play build number is obtained in [`codemagic.yaml`](./codemagic.yaml)

```bash
LATEST_GOOGLE_PLAY_BUILD_NUMBER=$(google-play get-latest-build-number --package-name 'io.codemagic.androidhelloworldapp')
```

Increment it and save to a variable (arbitrary name) if it's not empty.

```bash
LATEST_GOOGLE_PLAY_BUILD_NUMBER=$(google-play get-latest-build-number --package-name 'io.codemagic.androidhelloworldapp')
if [ -z LATEST_BUILD_NUMBER ]; then
    # fallback in case no build number was found from google play. Alternatively, you can `exit 1` to fail the build
    UPDATED_BUILD_NUMBER=$BUILD_NUMBER
else
    UPDATED_BUILD_NUMBER=$(($LATEST_GOOGLE_PLAY_BUILD_NUMBER + 1))
fi
```

Provide the created variable as gradlew argument properties for version code and version name:

```bash
./gradlew bundleRelease -PversionCode=$UPDATED_BUILD_NUMBER -PversionName=1.0.$UPDATED_BUILD_NUMBER
```

In [`build.gradle`](./app/build.gradle), the version code and name are read inside a function from the passed arguments

```java
def getMyVersionCode = { ->
    return project.hasProperty('versionCode') ? versionCode.toInteger() : -1
}

def getMyVersionName = { ->
    return project.hasProperty('versionName') ? versionName : "1.0"
}
```

and used inside the android config


```java
android {
    ...
    defaultConfig {
        ...
        versionCode getMyVersionCode()
        versionName getMyVersionName()
```

### Check other methods at:

- [`autoversioning_through_file`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_file) branch

    Read `versionCode` in `build.gradle` from a created `app/version.properties` file

- [`autoversioning_through_executable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_executable) branch

    Call the `google-play` tool directly from the `build.gradle` to get the build version code

- [`autoversioning_through_environment_variable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_environment_variable) branch

    Read a created environment variable with build number in `build.gradle`
