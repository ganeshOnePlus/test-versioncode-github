# 1) Create Identifier 
- Select type AppID's
- Select type App and Continue
- Enter bundle Id . eg. com.namespace.quizapp. Make sure this bundle identifier is same as in the Xcode->Targets->Signing Capabilities
- we can use different bundleId for different Signing configurations as well

# 2) Create Profiles
- For Development we have to selecte iOS App Development under 'Developent'
- then select the appID that we created earlier

- If we want to deploy our app to the app store or deploygate or testflight we need to create a provisining profile using iOS App distribution under 'Distribution'

# 3) Download the provision and paste in the root dir of the project name it according to your project

# 4) Export p12 file using keychains and also paste it in the root dir. It'll ask for a password (Don't forget the password). Rename it to ios_distribution.p12


# 5) Creating different build Configurations
- we might want to use different provisioning profile for dev, staging or release. Build configurations allows us to that.
- to create new build configuration select the project from side bar, then select the project under 'Project'. Create two different configurations, staging and release

# 6) Creating Schemes
- Schemas are like app with different build configuration set to them. For eg if we have 1 schemas and 3 different build configurations, we need to maually update the schemas if we want to run the app in staging or release mode. Instead we can create schemas for dev,staging,and release and set the build configurations accordingly. In this way we can just select the schema and run the app and it will run the app with the defined configurations.

- For Dev schema we are using dev build configuration
- For Staging schema we are using staging build config
- And For Release we are using release build config

# 7) Create app in App store connect
- Platform iOS
- Name: Name of the app
- bundleID: that we created earlier in step 1
- SKU: anything unique

This will give us a unique appleId that is assigned to our app(App->General->App Information-> General Info, Apple ID)
## NOTE: This apple ID is necessary to deploy our app to test flight

# 8) Generate Test Icons for ios
https://jpmallow.github.io/CopiCon/#/dashboard

To add Icons:
- To add icons open project in xcode
- select the project
- select images.xcassets
- select appicon
- drag images and paste into the respective placeholders


# 9) Update `fastlane/Appfile`
update accrding to your project
```
## iOS
app_identifier "com.namespace.quizapp" # The bundle identifier of your app
apple_id "" # Your Apple email address
team_id "" # team id
itc_team_id "" # itunes connect team id
```
- team_id can be found at the header of https://developer.apple.com/account/resources/certificates/list
- itunes connect team id (itc_team_id) can be found at https://appstoreconnect.apple.com/WebObjects/iTunesConnect.woa/ra/user/detail
# 10) update `fastlane/Fastfile`
update Fastfile according to the commented instructions


# 11) Create `.env` file
Create `.env` file as specified in `.env.sample`
```env
P12_PASSWORD=password ## <----- password that we set while creating p12 file
FASTLANE_APPLE_APPLICATION_SPECIFIC_PASSWORD= ##<---- Because we have enabled 2FA
```
# 11) Testing fastlane locally

```bash
bundle exec fastlane ios paste_lane_name_here
```

# 12) Deploy using github actions

First lets add the secrets in github repo.


# 13) Update `./github/workflows` according to the commented instructions

# 14) Add Github secrets
Some of the file needs to be encoded to do that

```bash
base64 encode /to/filename
```

```
# encoded .p12 file
P12_PROD
P12_STAG

# encoded .provision file
PROVISION_STAG
PROVISION_PROD


DOT_ENV_STAG
DOT_ENV_PROD

# encoded GoogleService-Info.plist
PLIST_STAG
PLIST_PROD

# encoded filename.keystore
KEY_STORE_PROD
KEY_STORE_STAG

# encoded key.json
PLAYSTORE_UPLOAD_KEY

# encoded google-services.json
GSERVICE_ANDROID_PROD
GSERVICE_ANDROID_STAG

```


