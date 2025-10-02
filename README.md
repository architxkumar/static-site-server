# Static Site Server

## Steps
### 1. VM Creation

1. Create new project on Google Cloud with your favourite name
2. Press on Create VM on the project dashboard
3. Enable the Compute Engine API
4. On the machine configuation option, select E2 instance. Select Machine type to custom, enable shared cores and tone down the slider to `0.25`.
5. Under the OS and storage option, change to latest version of Ubtuntu
6. For Data protection, select `No backups`
7. For Networking, enable HTTP Traffic. It's cruicial as nginx will serve HTTP traffic.
8. For security (optional), turn on secure boot.
9. Click on create instance

### 2. Server Setup
1. Generate SSH keypair on your local machine with the command
```bash
ssh-keygen -t ed22519 -c "your_mail@example.com"
```
> [!NOTE]
> Ensure the email is same as the one tied with Google Cloud account.
2. Skip the passphrase on the prompt.
3. Copy the contents of your public key (the one ending with `.pub`).
4. Click on the Metadata card under Compute Engine's Settings section.
5. Paste in the content under the `SSH Keys` header. This transfers the public key to all the created instances
6. Restart your instance
