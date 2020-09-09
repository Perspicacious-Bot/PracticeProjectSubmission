LAB: Google Cloud Fundamentals: Getting Started with Compute Engine
DURATION: 25 minutes.
OBJECTIVES: In this lab, you will learn how to perform the following tasks:

- Create a Compute Engine virtual machine using the Google Cloud Platform (GCP) Console.
- Create a Compute Engine virtual machine using the *gcloud* command-line interface.
- Connect between the two instances.

**Task 1: Create a virtual machine using the GCP Console**
STEPS:

1. To create a VM instance with name `my-vm-1`, with default machine type, region and zone. Boot disk, Debian GNU/Linux 9 (stretch) and firewall rule to Allow HTTP traffic, run:

   ```
   gcloud compute instances create my-vm-1 --machine-type "n1-standard-1"  --image-project  "debian-cloud" --image "debian-9-stretch-v20190213" --subnet "default" --tags http
   
   gcloud compute firewall-rules create http-firewall --action=ALLOW --direction=INGRESS --rules=tcp:80 --target-tags=http
   ```

   

**Task 2: Create a virtual machine using the *gcloud* command line.**
STEPS:

1. Display a list of zones in the region us-central1

   ```
   gcloud compute zones list | grep us-central1
   ```

2. Set Compute Zone. (Choose a zone from that list other than the zone to which Qwiklabs assigned you).

   ```
   gcloud config set compute/zone us-central1-b
   ```

3. To create a VM instance called my-vm-2 in that zone, execute this command:

   ```
   gcloud compute instances create "my-vm-2"  --machine-type "n1-standard-1"  --image-project "debian-cloud"  --image "debian-9-stretch-v20190213"  --subnet "default"
   ```

**Task 3: Connect between VM instances.**
STEPS:

1. View the two instances created by running:

   ```
   gcloud compute instances list
   ```

2. SSH into my-vm-2

   ```
   gcloud compute ssh my-vm-2
   ```

​        *Use the `ping` command to confirm that **my-vm-2** can reach **my-vm-1** over the network:*

3. Ping my-vm-1 through my-vm-2

   ```
   ping -c 3 my-vm-1
   ```

   

4. Use the **ssh** command to open a command prompt on **my-vm-1**:

   ```
   ssh my-vm-1
   ```

​          *If you are prompted about whether you want to continue connecting to a host with unknown authenticity, enter **yes** to confirm that you do.*

5. On my-vm-1, install the Nginx web server:

   ```
   sudo apt-get install nginx-light -y
   ```

6. Use the nano text editor to add a custom message to the home page of the web server:

   ```
   sudo nano /var/www/html/index.nginx-debian.html
   ```

7. Use the arrow keys to move the cursor to the line just below the `h1` header. Add text like this, and replace YOUR_NAME with your name:

   ```
   Hi from FRANK
   ```

8. Press **Ctrl+O** and then press **Enter** to save your edited file, and then press **Ctrl+X** to exit the nano text editor.

9. Confirm that the web server is serving your new page. At the command prompt on my-vm-1, execute this command:

   ```
   curl http://localhost/
   ```

10. To confirm that my-vm-2 can reach the web server on my-vm-1, at the command prompt on my-vm-2, execute this command: 

    ```
    curl http://my-vm-1/
    ```

    *The response will again be the HTML source of the web server's home page, including your line of custom text.*

11. Get the external IP address of my-vm-1

    ```
    gcloud compute instances list zone=us-central1-b
    ```

    Copy the External IP address for **my-vm-1** and paste it into the address bar of a new browser tab. You will see your web server's home page, including your custom text.