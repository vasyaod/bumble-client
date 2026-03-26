# Bumble Client

Small CLI project and OpenClaw skill for driving Bumble through a Remote Browser Service (RBS).

It includes:

- `skills/bumble/SKILL.md`: skill instructions and operating constraints
- `skills/bumble/scripts/bumble_client.py`: high-level Bumble flows such as auth, state inspection, matches, messages, sending, profiles, and photo export
- `skills/bumble/scripts/rbs_client.py`: low-level HTTP client for the Remote Browser Service API

## Requirements

- Python 3
- `requests`
- Access to the Remote Browser Service
- `AC_API_KEY` set when the RBS endpoint requires authentication

Optional environment variables:

- `AC_API_KEY`: bearer token for RBS requests
- `RBS_BASE_URL`: override the default RBS base URL

## Install

```bash
python3 -m pip install requests
```

## Usage

Run commands from the repository root:

Inspect current session state:

```bash
python3 skills/bumble/scripts/bumble_client.py state
python3 skills/bumble/scripts/bumble_client.py debug
python3 skills/bumble/scripts/bumble_client.py content
python3 skills/bumble/scripts/bumble_client.py snapshot
```

Start auth flow with a phone number:

```bash
python3 skills/bumble/scripts/bumble_client.py auth "<user_phone_number>"
```

Submit the SMS verification code:

```bash
python3 skills/bumble/scripts/bumble_client.py sms_code "<6-digit-code>"
```

List matches:

```bash
python3 skills/bumble/scripts/bumble_client.py matches
```

List likes / Beeline admirers:

```bash
python3 skills/bumble/scripts/bumble_client.py likes
```

Read messages for a match:

```bash
python3 skills/bumble/scripts/bumble_client.py messages "Kritika"
```

Send a message:

```bash
python3 skills/bumble/scripts/bumble_client.py send "Kritika" "Hello there"
```

Fetch a profile:

```bash
python3 skills/bumble/scripts/bumble_client.py profile "Kritika"
```

Export profile photos:

```bash
python3 skills/bumble/scripts/bumble_client.py photos "Anya" "/absolute/output/dir"
```

Low-level tap helper:

```bash
python3 skills/bumble/scripts/bumble_client.py tap "Continue with other methods"
```

## ClawHub Packaging

The publishable skill bundle lives in `skills/bumble`.

## Session Behavior

- Reuses the stored `bumble` session when possible
- Starts from `https://bumble.com/app`
- Sets location to San Francisco after session start
- Avoids re-auth unless Bumble is clearly on logged-out or SMS-verification screens
- Does not intentionally log out

## Notes

- The auth flow expects the phone number to be provided by the user at runtime.
- SMS verification may still require manual handling for CAPTCHA or passkey-related screens.
- Match and message operations depend on the current Bumble DOM and text structure, so UI changes can break selectors/parsing.
