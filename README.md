name: MVP Live Deployment

on:
  push:
    branches: [ "main" ]

# Grant GITHUB_TOKEN the permissions needed for GitHub Pages
permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  # STEP 1: BUILD & PACKAGE
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      
      - name: Simulate Build Process
        run: echo "Building the MVP..."
      
      # This effectively 'zips' your website files so they are ready to ship
      - name: Upload artifact
        uses: actions/upload-pages-artifact@v3
        with:
          path: '.'

  # STEP 2: TEST (Must pass before we deploy!)
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3
      
      - name: Quality Assurance Check
        run: |
          if grep -q "Startup" index.html; then
            echo "Test Passed: Branding found!"
          else
            echo "Test Failed: Branding missing!"
            exit 1 
          fi

  # STEP 3: DEPLOY TO LIVE INTERNET
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    needs: test
    runs-on: ubuntu-latest
    steps:
      - name: Deploy to GitHub Pages
        id: deployment
        uses: actions/deploy-pages@v4
