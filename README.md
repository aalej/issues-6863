# Repro for issue 6863

## Steps to reproduce

1. Run `npx create-next-app@latest`
2. Run `firebase init hosting --project project_id`
3. Run `firebase init hosting:github --project project_id`
   - Enter a GitHub repository
   - Choose all defaults
4. Modify the `firebase-hosting-merge.yaml` file to include `FIREBASE_CLI_EXPERIMENTS: webframeworks`

```
jobs:
  build_and_deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: FirebaseExtended/action-hosting-deploy@v0
        with:
          repoToken: '${{ secrets.GITHUB_TOKEN }}'
          firebaseServiceAccount: '${{ secrets.FIREBASE_SERVICE_ACCOUNT_FIR_SUPPORT_TESTPROJ }}'
          channelId: live
          projectId: fir-support-testproj
        env:
          FIREBASE_CLI_EXPERIMENTS: webframeworks
```

5. Push to the GitHub repository
   - `git add .`
   - `git commit -m "Some message"`
   - `git push origin main`
6. GitHub action raises an error when deploying the Next.js application

```
  ***
    "status": "error",
    "error": "Could not find the next executable."
  ***
```

see https://github.com/aalej/issues-6863/actions/runs/8233857278/job/22514273947

## Notes:

Deploying locally using firebase-tools v13.4.1 works fine


