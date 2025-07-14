# Deploy to Rezoleo via SFTP

This GitHub Action allows you to deploy content to Rezoleo's Hippolyte server via SFTP using `lftp` and an SSH private key or a password. It automates the deployment process while removing the `.git` folder as well as the `.gitignore`. It deploys the content to the `writable` folder on the server.

## Features
- Checks if the inputs are correct (either a key or a password must be provided).
- Installs `lftp` for SFTP operations.
- Deploys content securely using an SSH private key or a password.

## Inputs

### `sftp-user`
**Required**

The username for the SFTP server.

### `sftp-password`
**Partially optional**

The password for the SFTP server. This password is used to authenticate the deployment process.

### `sftp-key`
**Partially optional**

The SSH private key for the SFTP server. This key is used to authenticate the deployment process.

You must ask a Rezoleo administrator to add your dedicated public key to your user on the server.

> [!IMPORTANT] 
> You must either provide a key, or a password, not both!

## Usage

To use this action, include it in your workflow file. Below is an example:

```yaml
name: Deploy to Rezoleo

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Deploy to Rezoleo
        uses: rezoleo/rezoleo-deploy-action@v1
        with:
          sftp-user: ${{ secrets.SFTP_USER }}
          # Use only one of the following lines :
          #sftp-password: ${{ secrets.SFTP_PASSWORD }}
          sftp-key: ${{ secrets.SFTP_KEY }}
```

## Secrets

You need to define the following secrets in your repository:

- `SFTP_USER`: The username for the SFTP server.
- `SFTP_PASSWORD`: The password for the SFTP server.
- `SFTP_KEY`: The SSH private key for the SFTP server.

## Notes

- The SSH private key is written to a file named `id_rsa` during the deployment process.
- Caution : The upload process will completely overwrite the content in the `writable` folder on the server, except the `.git` folder and what is included in the `.gitignore` file.

> [!WARNING] 
> Include directives present in the `.gitignore` files (lines starting with `!`) are not respected!
