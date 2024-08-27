# cpanel-react

## Manual Deployment

### Step 1: Install FtP Client

You can use any FTP client to upload files to the server. In this guide, we will use FileZilla.

#### 1. Download and install FileZilla from the [official website](https://filezilla-project.org/).

#### 2. Open FileZilla and go to `File > Site Manager`.

#### 3. Click on `New Site` and enter the following details:

- Host: Your server IP address or domain name
- Port: 21
- Protocol: FTP - File Transfer Protocol
- Encryption: Use explicit FTP over TLS if available
- Logon Type: Normal
- User: Your cPanel username
- Password: Your cPanel password

#### 4. Click on `Connect` to establish a connection to the server.

### Step 2: Upload Files

#### 1. Once connected, you will see two panes. The left pane shows the files on your local machine, and the right pane shows the files on the server.

#### 2. Navigate to the directory where you want to upload your website files on the server. This is typically the `public_html` directory.

#### 3. In the left pane, navigate to the directory where your website files are located.

#### 4. Select the files and folders you want to upload and drag them to the right pane to upload them to the server.

### Step 3: Verify Website

#### 1. Once the files are uploaded, you can access your website using your domain name in a web browser.

#### 2. If you encounter any issues, check the file permissions and ensure that the files are uploaded to the correct directory.

## CI/CD Pipeline Deployment

Below is the sample CI/CD pipeline for deploying the frontend on push to the main branch. The pipeline uses SSH to connect to the server and deploy the code.

```yaml
on:
  push:
    branches:
      - main
name: 🚀 Deploy
jobs:
  web-deploy:
    name: 🎉 Deploy
    runs-on: ubuntu-latest
    steps:
      - name: 🚚 Get latest code
        uses: actions/checkout@v3

      - name: Use Node.js 20
        uses: actions/setup-node@v2
        with:
          node-version: "20"

      - name: 🔨 Install Packages
        run: |
          npm install
          npm run build

      - name: 📂 Sync files
        uses: SamKirkland/FTP-Deploy-Action@4.1.0
        with:
          server: ${{ secrets.SERVER }}
          username: ${{ secrets.USER }}
          password: ${{ secrets.PWD }}
          server-dir: ${{ secrets.DIR }}
          local-dir: ./dist/
```
