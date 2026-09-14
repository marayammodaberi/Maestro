# Gobolia automation

This repo contains the two requested UI automation flows in English:

1. Admin sign-up and login
2. Booking (appointment)

## Tool

Maestro was used because it is fast for black-box Android UI automation.

## Requirements

- Android phone or emulator connected via ADB
- App installed on the device
- Maestro installed

```bash
curl -Ls "https://get.maestro.mobile.dev" | bash
export PATH="$PATH:$HOME/.maestro/bin"
export PATH="$PATH:$HOME/Library/Android/sdk/platform-tools"
```

## Device check

```bash
adb devices
```

The connected device should appear as `device`.

## Test values

- Phone number: `09304216502`
- OTP: `112000`
- App package: `com.practicalidea.gobolia.dev`

## Run the flows

From this folder:

```bash
cd /Users/maryam/Documents/GitHub/Maestro/automation

# Admin sign-up and login
maestro test -e ANDROID_SERIAL=R8YW70J7TMV admin_signup_login.yaml

# Booking flow
maestro test -e ANDROID_SERIAL=R8YW70J7TMV booking_flow.yaml

# Therapist login and promise creation
maestro test -e ANDROID_SERIAL=R8YW70J7TMV therapist_login_create_promise.yaml
```

## Files

- `admin_signup_login.yaml`
- `booking_flow.yaml`
- `therapist_login_create_promise.yaml`

## Notes

- This is a black-box test, so it depends on the visible English UI text.
- If the actual app text differs slightly, update the selector strings in the YAML files.
- If the package name is different in your build, update `appId` at the top of each YAML file.
