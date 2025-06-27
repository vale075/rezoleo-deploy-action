# Deploy to Rezoleo via SFTP

This GitHub Action allows you to deploy content to Rezoleo's Hippolyte server via SFTP using `lftp` and an SSH private key. It automates the preparation of a clean folder and the deployment process. It deploys the content to the `writable` folder on the server.

## Features
- Prepares a clean folder for deployment.
- Installs `lftp` for SFTP operations.
- Deploys content securely using an SSH private key.

## Inputs

### `sftp-key`
**Required**

The SSH private key for the SFTP server. This key is used to authenticate the deployment process.

You must ask a Rezoleo administrator to add your dedicated public key to your user on the server.

### `sftp-user`
**Required**

The username for the SFTP server.

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
          sftp-key: ${{ secrets.SFTP_KEY }}
          sftp-user: ${{ secrets.SFTP_USER }}
```

## Secrets

You need to define the following secrets in your repository:

- `SFTP_KEY`: The SSH private key for the SFTP server.
- `SFTP_USER`: The username for the SFTP server.

## Scripts

This action uses the following scripts:
- `scripts/prepare-upload.sh`
- `scripts/deploy.sh`

## Notes

- A new folder named `clean_upload` is created during the deployment process.
- The SSH private key is written to a file named `id_rsa` during the deployment process.
- Caution : the upload process will overwrite completely the content in the `writable` folder on the server.