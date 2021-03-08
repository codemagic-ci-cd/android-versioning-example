This is an example of a simple android application which uses different ways to specify build's `versionCode`.

In particular, the version code is the incremented latest build number fetched from Google Play Developer API throught [`google-play`](https://github.com/codemagic-ci-cd/cli-tools/tree/master/docs/google-play#google-play) Codemagic CLI Tool.

### Pass build number through a file:

The latest Google Play build number is obtained in [`codemagic.yaml`](./codemagic.yaml)

```bash
LATEST_GOOGLE_PLAY_BUILD_NUMBER=$(google-play get-latest-build-number --package-name 'io.codemagic.androidhelloworldapp')
```

Increment it and echo it to `app/version.properties` (arbitrary name) file if it's not empty.

```bash
if [ -z LATEST_BUILD_NUMBER ]; then
# fallback in case no build number was found from google play. Alternatively, you can `exit 0` to fail the builds
    echo "VERSION_CODE=$BUILD_NUMBER" > app/version.properties
else
    echo "VERSION_CODE=$(($LATEST_GOOGLE_PLAY_BUILD_NUMBER + 1))" > app/version.properties
fi
```

In [`build.gradle`](./app/build.gradle), the version is read inside a function from the written above `app/version.properties`

```java
def getVersionCode() {
    def propertiesFile = file('version.properties')

    if (!propertiesFile.canRead()) {
        throw new GradleException("Could not read " + propertiesFile.name)
    }

    Properties properties = new Properties()
    properties.load(new FileInputStream(propertiesFile))

    return properties['VERSION_CODE'].toInteger()
}
```

and is used inside the android config:

```java
android {
    def code = getVersionCode()

    ...
    defaultConfig {
        ...
        versionCode code
        versionName "1.0." + code.toString()
```

### Check other methods at:

-  [`master`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/master) branch

    Pass the desired version code with gradlew argument properties

- [`autoversioning_through_executable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_executable) branch

    Call the `google-play` tool directly from the `build.gradle` to get the build version Code

- [`autoversioning_through_environment_variable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_environment_variable) branch

    Read a created environment variable with build number in `build.gradle`