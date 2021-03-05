This is an example of a simple android application which uses different ways to specify build's `versionCode`.

In particular, the version code is the incremented latest build number fetched from Google Play Developer API throught `google-play` Codemagic CLI Tool.

Check `codemagic.yaml` and `app/build.gradle` for the details.

Following methods are demonstrated:

-  [`master`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/master) branch

    Pass the desired version code with gradlew argument properties

- [`autoversioning_through_file`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_file) branch

    Read `versionCode` in `build.gradle` from a created `app/version.properties` file

- [`autoversioning_through_executable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_executable) branch

    Call the `google-play` tool directly from the `build.gradle` to get the build version Code

- [`autoversioning_through_environment_variable`](https://github.com/codemagic-ci-cd/android-versioning-example/tree/autoversioning_through_environment_variable) branch

    Read a created environment variable with build number in `build.gradle`