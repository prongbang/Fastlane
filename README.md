# Fastlane documentation

## Installation

Make sure you have the latest version of the Xcode command line tools installed:

```
xcode-select --install
```

Install _fastlane_ using
```
[sudo] gem install fastlane -NV
```
or alternatively using `brew cask install fastlane`

# Available Actions
## Android
### android test
```
fastlane android test
```
Runs all the tests
### android alpha
```
fastlane android alpha
```
Submit a new Alpha Build to Play Store
### android beta
```
fastlane android beta
```
Submit a new Beta Build to Crashlytics Beta
### android deploy
```
fastlane android deploy
```
Deploy a new version to the Google Play

----

### Appfile
```gradle
json_key_file("/<Path>/api-access-key.json") # Path to the json secret file - Follow https://docs.fastlane.tools/actions/supply/#setup to get one
package_name("com.package.name") # e.g. com.krausefx.app
```
### Fastfile
```ruby
# This file contains the fastlane.tools configuration
# You can find the documentation at https://docs.fastlane.tools
#
# For a list of all available actions, check out
#
#     https://docs.fastlane.tools/actions
#

# Uncomment the line if you want fastlane to automatically update itself
# update_fastlane

default_platform(:android)

platform :android do
  desc "Runs all the tests"
  lane :test do
    gradle(task: "test")
  end

  desc "Submit a new Alpha Build to Play Store"
  lane :alpha do
    gradle(task: 'clean', project_dir: "android/") # Clean the Gradle project
    gradle(task: "assemble", build_type: "Release", project_dir: "android/") # Build the Release APK
    supply(track: "alpha", apk: "android/app/build/outputs/apk/app-release.apk") # Upload the APK to the Play Store (alpha)
  end

  desc "Submit a new Beta Build to Crashlytics Beta"
  lane :beta do
    gradle(task: "clean assembleRelease")

    # crashlytics

    upload_to_play_store(track: 'beta')

    # sh "your_script.sh"
    # You can also use other beta testing services here
  end

  desc "Deploy a new version to the Google Play"
  lane :deploy do
    gradle(task: "clean assembleRelease")
    upload_to_play_store
  end
end
```

### Android Setup
[https://docs.fastlane.tools/getting-started/android/setup/#setting-up-supply](https://docs.fastlane.tools/getting-started/android/setup/#setting-up-supply)
