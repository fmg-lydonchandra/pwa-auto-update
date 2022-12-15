### Quick Start

1. Powershell 
   1. `npm run build; serve -s build -p 3005`
2. Open `http://localhost:3005` in browser
3. Install as App
4. Edit some files, then re-build
   1. `npm run build; serve -s build -p 3005`
5. Watch App auto-updates itself

### Notes:
1. For demo purpose, the update check is done every 10 seconds
2. Auto updater hinges on the addition of the following `console.log` statement in `service-worker.ts`
   1. ``` 
      // Any other custom service worker logic can go here.
      const buildVersion = (): void => {
      // eslint-disable-next-line no-console
      console.log("Build version: ", process.env.REACT_APP_BUILD_VERSION);
      };
      buildVersion();
      ```
   2. `.env.REACT_APP_BUILD_VERSION` is updated automatically on every `npm run build`,
      see `.\package.json`
3. The tricky bit is when this auto-updater code is added AFTER an app is installed 
   1. Timeline:
      1. Build app WITHOUT auto-updater, called `app-v0`
      2. Install & run `app-v0`
      3. Edit some files, add auto-updater bit, rebuild as `app-v1`
      4. `app-v0` is still running, restart it
      5. `app-v0` detects that an update `app-v1` is available, downloads and installs `app-v1`
         1. Close `app-v0` to activate `app-v1`
         2. Re-open app, `app-v1` runs now
         3. auto-updater should work as expected now

