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
    "error": "Unable to detect the web framework in use, check firebase-debug.log for more info.\n\n\u001b[1mDocumentation:\u001b[22m https://firebase.google.com/docs/hosting/frameworks/frameworks-overview\n\u001b[1mFile a bug:\u001b[22m https://github.com/firebase/firebase-tools/issues/new?template=bug_report.md\n\u001b[1mSubmit a feature request:\u001b[22m https://github.com/firebase/firebase-tools/issues/new?template=feature_request.md\n\nWe'd love to learn from you. Express your interest in helping us shape the future of Firebase Hosting: https://goo.gle/41enW5X"
  ***
```

see https://github.com/aalej/issues-6863/actions/runs/8233857278/job/22514273947

## Notes:

Deploying locally using firebase-tools v13.4.1 works fine
