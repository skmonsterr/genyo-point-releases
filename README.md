# Genyo Point Automation
<!-- GENYO_LATEST_VERSION: v1.0.9 -->

Genyo Point Automation is a Windows desktop app for running and scheduling Genyo clock-in and clock-out operations from your computer.

The app is designed for daily use with local settings, Safe Mode testing, Real Mode execution, background scheduling, Windows notifications, activity logs, and automatic updates through GitHub Releases.

## What You Can Do

With the app you can:

- configure your Genyo access details;
- test clock-in and clock-out in Safe Mode before submitting a real record;
- run Clock In or Clock Out manually when needed;
- keep the Scheduler active in the background;
- see the last and next scheduled operations;
- receive Windows notifications when scheduled operations run;
- check for and install app updates.

## Download Files

Open the latest release page:

```text
https://github.com/skmonsterr/genyo-point-releases/releases/latest
```

Current latest version: `v1.0.9`.

Download these two files:

```text
genyo-point-automation-setup-v<version>-win32-x64.exe
genyo-self-signed-code-signing.cer
```

You do not need to download `latest.yml` or `.blockmap`. Those files are used internally by the auto-update system.

## Before Installing

Run PowerShell as Administrator:

1. Open the Start menu.
2. Search for `PowerShell`.
3. Right-click **Windows PowerShell**.
4. Click **Run as administrator**.

The examples below assume both files are in your Downloads folder and use the current latest version shown above.

### 1. Validate the Setup Before Trusting the Certificate

Run:

```powershell
Get-AuthenticodeSignature "$env:USERPROFILE\Downloads\genyo-point-automation-setup-v1.0.9-win32-x64.exe" | Format-List
```

Expected result before installing the certificate:

```text
SignerCertificate      : [certificate information]
TimeStamperCertificate :
Status                 : UnknownError
StatusMessage          : A certificate chain processed, but terminated in a root certificate which is not trusted by the trust provider.
Path                   : C:\Users\<your-user>\Downloads\genyo-point-automation-setup-v1.0.9-win32-x64.exe
SignatureType          : Authenticode
IsOSBinary             : False
```

The important point is: before trusting the `.cer`, the file may be signed but Windows does not trust the certificate yet.

### 2. Install the Public Certificate

Run both commands in the same Administrator PowerShell window:

```powershell
Import-Certificate `
  -FilePath "$env:USERPROFILE\Downloads\genyo-self-signed-code-signing.cer" `
  -CertStoreLocation "Cert:\CurrentUser\Root"
```

Expected result:

```text
PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\Root

Thumbprint                                Subject
----------                                -------
<certificate-thumbprint>                  CN=Genyo Point Automation Test
```

Then run:

```powershell
Import-Certificate `
  -FilePath "$env:USERPROFILE\Downloads\genyo-self-signed-code-signing.cer" `
  -CertStoreLocation "Cert:\CurrentUser\TrustedPublisher"
```

Expected result:

```text
PSParentPath: Microsoft.PowerShell.Security\Certificate::CurrentUser\TrustedPublisher

Thumbprint                                Subject
----------                                -------
<certificate-thumbprint>                  CN=Genyo Point Automation Test
```

### 3. Validate the Setup Again

Run:

```powershell
Get-AuthenticodeSignature "$env:USERPROFILE\Downloads\genyo-point-automation-setup-v1.0.9-win32-x64.exe" | Format-List
```

Expected result after installing the certificate:

```text
SignerCertificate      : [certificate information]
TimeStamperCertificate :
Status                 : Valid
StatusMessage          : Signature verified.
Path                   : C:\Users\<your-user>\Downloads\genyo-point-automation-setup-v1.0.9-win32-x64.exe
SignatureType          : Authenticode
IsOSBinary             : False
```

The important point is: `Status` should be `Valid` before you run the installer.

## Install the App

After the signature is valid:

1. Run `genyo-point-automation-setup-v<version>-win32-x64.exe`.
2. Choose the installation folder when Windows asks.
3. Finish the installation.
4. Open **Genyo Point Automation** from the Windows shortcut.

If Windows still shows a SmartScreen warning, confirm that the signature status is `Valid` and that the file was downloaded from this repository release page.

## First Setup

Open the app and click **Settings**.

Fill in the required fields:

- **Company code**: your Genyo company code.
- **Access Number**: your Genyo access number.
- **2Captcha API key**: the API key used for reCAPTCHA solving.

Configure the schedule:

- **Clock In**: scheduled clock-in time.
- **Clock Out**: scheduled clock-out time.
- **Weekdays**: days when both scheduled operations should run.

Browser and notification settings:

- **Windows notifications**: keep enabled if you want Windows notifications.
- **Headless browser**: keep enabled for normal background use.
- **Browser channel**: choose Microsoft Edge or Google Chrome.
- **Browser executable path**: optional. Only fill this if you need to use another Chromium-based browser executable. When filled, it overrides Browser channel.

Click **Save** after changing settings.

## Safe Mode and Real Mode

The app has two execution modes.

### Safe Mode

Use **Safe Mode** to test. The app runs the flow but blocks the final confirmation click, so no time record is submitted.

Recommended use:

- after the first setup;
- after changing credentials;
- after changing browser settings;
- whenever you want to confirm the flow before using Real Mode.

### Real Mode

Use **Real Mode** only when you want the app to submit the real Genyo clock-in or clock-out action.

The Scheduler must run in Real Mode. If Safe Mode is active, the app blocks Scheduler start to avoid a wrong background setup.

## Daily Use

There are two main ways to use the app.

### Manual Operation

Use this when you want to run Clock In or Clock Out manually.

1. Open the app.
2. Confirm that Settings are complete.
3. Test with Safe Mode if needed.
4. Switch to Real Mode.
5. Click **Run Clock In (Real Mode)** or **Run Clock Out (Real Mode)**.
6. Check **Activity** for the operation status.

### Scheduler in Background

Use this when you want the app to wait for the configured times.

1. Open the app.
2. Confirm that Settings are complete.
3. Keep **Headless browser** enabled for background use.
4. Switch to Real Mode.
5. Click **Start scheduler**.
6. The app will move to the Windows tray and keep running in the background.

You only need to start the Scheduler once. It keeps running until you manually stop it from the app or from the Windows tray.

The main screen shows:

- whether the Scheduler is active;
- the last Clock In and Clock Out operation;
- the next scheduled Clock In and Clock Out operation;
- whether the last operation succeeded or failed.

## Windows Tray

When the Scheduler is active, the app can stay in the Windows tray.

From the tray you can:

- reopen the app;
- stop the Scheduler;
- quit the app.

If the computer restarts, the app tries to restore the Scheduler automatically if it was active before and was not manually stopped.

## Updates

The app can check for new versions from GitHub Releases.

Use **Check for updates** in the app.

Before installing an update:

1. Stop the Scheduler.
2. Wait for any running operation to finish.
3. Download and install the update from the app when prompted.
4. Reopen the app and confirm Settings.

Normal updates preserve local Settings, logs, and Scheduler state.

## Local Files

The app stores local data in your Windows user profile, usually under:

```text
AppData\Roaming\genyo-point-automation
```

Important folders:

- `config\genyo-config.txt`: local app settings;
- `logs\`: user-friendly and technical logs;
- `state\`: Scheduler state.

Do not share `genyo-config.txt`. It contains sensitive access details.

## Good Practices

- Keep your Genyo access details private.
- Keep the 2Captcha API key private.
- Use Safe Mode after changing Settings.
- Keep Headless browser enabled for Scheduler use.
- Do not close the browser while an operation is running.
- Stop the Scheduler before installing updates.
- Keep the computer on and connected to the internet at scheduled times.

## Common Issues

### Signature is not valid before installing the certificate

This is expected with the current self-signed certificate flow. Install `genyo-self-signed-code-signing.cer` into `CurrentUser\Root` and `CurrentUser\TrustedPublisher`, then validate the setup again.

### Signature is still not valid after installing the certificate

Check that:

- PowerShell was opened as Administrator;
- the `.cer` file came from the same release page;
- the setup file name and path are correct;
- you ran both `Import-Certificate` commands.

### Scheduler does not start

Check that:

- Settings are complete;
- the app is in Real Mode;
- Clock In and Clock Out times are configured;
- the configured browser is available on Windows.

### Browser does not open

Open **Settings** and check Browser channel or Browser executable path.

For most Windows users, Microsoft Edge works with:

```text
Browser channel: Microsoft Edge
Browser executable path: empty
```

### Operation failed

Check **Activity** in the app. It shows simplified messages for normal use.

If needed, run the same operation in Safe Mode first.

### Browser was closed during execution

The app treats this as an interrupted operation. Run the operation again when ready.




