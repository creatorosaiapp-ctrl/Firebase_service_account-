name: Deploy Functions to Firebase

on:
  push:
    branches:
      - main
    paths:
      - 'functions/**'

jobs:
  deploy:
    name: Deploy Firebase Functions
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'

      - name: Install Dependencies
        run: |
          cd functions
          npm install

      - name: Install Firebase CLI
        run: npm install -g firebase-tools

      - name: Deploy Functions
        run: |
          echo '${{ secrets.FIREBASE_SERVICE_ACCOUNT }}' > $HOME/gcloud-service-key.json
          export GOOGLE_APPLICATION_CREDENTIALS="$HOME/gcloud-service-key.json"
          firebase deploy --only functions --project studio-9763012102-f2834
