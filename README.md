# ManalEx


name: MVP Deployment Pipeline

# This triggers the pipeline whenever you push code to the main branch
on:
  push:
    branches: [ "main" ]

jobs:
  # STEP 1: BUILD
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      
      - name: Simulate Build Process
        run: |
          echo "Compiling assets..."
          echo "Minifying JavaScript..."
          echo "Build Complete!"

  # STEP 2: TEST (Runs only if Build succeeds)
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      
      - name: Quality Assurance Check
        run: |
          # This script checks if your HTML file contains the word "Startup"
          if grep -q "Startup" index.html; then
            echo "Test Passed: Branding found!"
          else
            echo "Test Failed: Branding missing!"
            exit 1 
          fi

  # STEP 3: DEPLOY (Runs only if Test succeeds)
  deploy:
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Simulate Deployment
        run: |
          echo "Connecting to server..."
          echo "Uploading files..."
          echo "🚀 DEPLOYMENT SUCCESSFUL! Your app is live."
