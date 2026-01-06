name: Security Leak Demo
on: [push]
  
jobs:
  leak-job:
    runs-on: ubuntu-latest
    steps:   
      - name: Checkout code
        uses: actions/checkout@v4
        
      - name: Attempting to print secret (Masked)
        run: |
            MY_TOKEN="super-secret-123"
            echo "The token is $MY_TOKEN" # This prints in plain text!

      - name: Mask a dynamic value
        run: |
          NEW_TOKEN=$(tr -dc 'a-zA-Z0-9' </dev/urandom | head -c 16)
          cat $NEW_TOKEN
          echo "::add-mask::$NEW_TOKEN"
          echo "The token is $NEW_TOKEN" # Output: The token is ***
