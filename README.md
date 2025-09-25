# CS554-lbs-to-kgs

A lightweight, stateless HTTP API that converts pounds to kilograms. Implemented with Node.js/Express, reverse proxied by Nginx, and ran on a single AWS e2e instance.

## Use instructions

To use the API, simply call the public endpoint with an HTTP GET in this format:

http://18.222.139.95/convert?lbs=

With the integer at the end you wish to convert.

## Setup instructions

### Install

There is no installation required to use this endpoint. It's entirely public (as long as the instance is running).

### Test

You may perform a fast GET test by using curl.

1. Install curl: `sudo apt update && sudo apt install curl`

2. Call the endpoint: `curl 'http://18.222.139.95/convert?lbs=[integer]`

Response should be in the following format if successful (status code 200):
```
{
    "lbs": <int>,
    "kg": <int>,
    "formula": "kg = lbs * 0.45359237"
}
```

If the connection fails, the instance is likely not running.

## Cleanup Protocol

After the instance is no longer in use, perform the following cleanup protocol:

Temporary stop:

1. Create a snapshot of the current EB volume.
2. Stop the current instance.
3. Delete the current EB volume and any unnessecary volume snapshots.

Permanent stop (instance service is not needed any longer):

1. Terminate the instance.
2. Delete current EB volume and prune all snapshots.
3. Remove your SSH key from AWS (and your local machine).

## Setup Steps

To create a new instance that provides this service:

1. Launch a new instance from the AWS console. Configure it to use Ubuntu LTS as it's latest.
2. Create and name an RSA key.
3. Create a new security group with the following rules:
   
   Allow SSH traffic from: My IP

4. Launch the instance. Note it's public IP instance.

On LINUX:

5. Move your key to a notable directory:
```
mkdir ~/.ssh
mv <PATH_TO_KEY>/mysshkey.pem ~/.ssh
cd ~/.ssh
```

6. Use the key to SSH to your instance:
```
chmod 400 MyKeyPair.pem
ssh -i MyKeyPair.pem ubuntu@<PUBLIC_IP>
```
You should now be user `ubuntu@<PRIVATE_IP>`.

7. Install dependencies:
```
sudo apt update
sudo apt install -y nodejs npm
sudo apt install -y nginx
npm init -y
npm install express morgan
```

8. Create service file:
```
mkdir -p ~/p1 && cd ~/p1
touch 'server.js'
```

9. Run `sudo vim server.js` and copy the entire contents of src/server.js into it. Type `:wq` to save and quit.
10. Create a systemd unit with `touch /etc/systemd/system/p1.service`. Copy the entire contents of deploy-code/systemd/p1.service into it.
11. Reload and enable systemd:
```
sudo systemctl daemon-reload
sudo systemctl enable --now p1
sudo systemctl status p1 --no-pager
```
12. Go back to the AWS Console, enter your security groups and create a new inbound rule:

    Type: HTTP
    Port: 80
    Source: 0.0.0.0/0
    
13. Create a nginx config file at `/etc/nginx/sites-available/p1` (note `p1` does not have an extension). Copy the entire contents of deploy-code/nginx/p1.conf into it.
14. Enable nginx:
```
sudo ln -sf /etc/nginx/sites-available/p1 /etc/nginx/sites-enabled/p1 # Create a symlink between available and enabled sites.
sudo systemctl enable --now nginx
```
   If you get an error, run `sudo nginx -t` to test. It should provide an error message for trouble shooting.
   Otherwise, reload systemctl with `sudo systemctl reload nginx`

Congratulations, you have done what I did to make this project.
