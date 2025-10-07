# Static Site Server

These are the documented steps I followed while working on the [Static Site Server](https://roadmap.sh/projects/static-site-server) DevOps Project.

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
7. SSH to your instance by running the folllowing command
    ```bash
    ssh -i /path/to/private-key username@hostaddress
    ```

### 3. Nginx Installation & Setup
1. Install the nginx by running the following command on Debian based system
    ```bash
    sudo apt update
    sudo apt install nginx
    ```
2. Check if nginx service is running by running the following command
    ```bash
    sudo systemctl status nginx
    ```
    > [!NOTE]
    > If it shows running, then you are good to go else you might have to enable nginx service manually
3. Navigate to root directory to create a basic html webpage that is to be served
    > [!NOTE]
    > This isn't the final html webpage that will be served
4. Create a directory `data/www` on your root path.
5. Create a file `index.html` and add a basic webpage of your choice
    > [!TIP]
    > Feel free to explore [HTML Boilerplate](https://htmlboilerplates.com/) website.
6. Edit the nginx.conf file to serve your index.html by referencing [beginner's guide](   https://nginx.org/en/docs/beginners_guide.html#static) on nginx docs.
7. Restart nginx by running the following the command
    ```bash
    nginx -s reload
    ```
8. Visit the webpage by entering in the external IP Address of your VM.
    > [!WARNING]
    > If you are seeing the same setup page try deleting `/etc/nginx/sites-enabled/` and restarting. Else, if you see a 403 forbidden webpage then it means that nginx found the webpage but doesn't have sufficient permission to read it. Look at this [Neos article](https://discuss.neos.io/t/solved-nginx-403-directory-listing-forbidden/5749/4)
## 3. Setting up rsync
1. Ensure that local and remote machine have rsync installed
2. Create your new webpage locally on your machine.
3. In order to sync directory between local and host machine, run the following commands
    ```bash
     rsync -avz -e "ssh -i /path/to/private-key" /path/to/local/directory hostname@hostaddress:/path/on/remote/directory
    ```
    > [!WARNING]
    > Ensure the user on remote has permission to write file in the directory. Also be cautious of the [trailing slash difference](https://stackoverflow.com/questions/18158248/should-i-put-trailing-slash-after-source-and-destination-when-copy-folders), it might have unwanted consequences.
4. Restart nginx and enjoy!

