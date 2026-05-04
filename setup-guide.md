Fortress Setup Guide: Generating VPN Certificates
To use the AWS Client VPN in this project, you must generate digital certificates to handle mutual authentication. I use OpenVPN Easy-RSA to create a Certificate Authority (CA) and the necessary server/client keys.

1. Generate Certificates (Using CloudShell)
The easiest way to do this without installing software on your local machine is using AWS CloudShell.

Open AWS CloudShell from the AWS Console.

Run the following commands to use a pre-configured Docker container that handles the heavy lifting:

Bash
# Create a folder for your keys
mkdir vpn-keys && cd vpn-keys

# Run the Easy-RSA tool via Docker
docker run -v $(pwd):/vpn -it discoverone/easyrsa-pki
Inside the container, follow the prompts to:

Initialize the PKI.

Build the CA (Certificate Authority).

Generate the Server certificate and key.

Generate the Client certificate and key.

2. Import to AWS Certificate Manager (ACM)
Once your files (ca.crt, server.crt, server.key, client1.crt, client1.key) are generated in your CloudShell folder, you need to import them so AWS can see them.

Run these two commands in CloudShell:

Import Server Certificate:

Bash
aws acm import-certificate --certificate fileb://server.crt \
--private-key fileb://server.key \
--certificate-chain fileb://ca.crt
Import Client Certificate:

Bash
aws acm import-certificate --certificate fileb://client1.crt \
--private-key fileb://client1.key \
--certificate-chain fileb://ca.crt

3. Copy the ARNs
Go to the AWS Certificate Manager (ACM) console.

Look for your two newly imported certificates.

Copy the ARN for each (it starts with arn:aws:acm...).

Paste these ARNs into the CloudFormation Parameters when launching the stack.

4. Connecting to the VPN
After CloudFormation finishes:

Download the Client Configuration File from the Client VPN Endpoints console.

Open the file in a text editor and append the contents of your client1.crt and client1.key between <cert> and <key> tags.

Import the file into the AWS VPN Client or OpenVPN Connect.

Connect! You can now SSH into your private instance using its private IP address.
