# Free Certificate from Let's Encrypt

## Installing Certbot

Copy-paste and run the following command on your terminal to install certbot. See.[Original Documentation](https://certbot.eff.org/docs/install.html)

```
sudo apt-get update && sudo apt-get install -y software-properties-common && sudo add-apt-repository -y universe && sudo add-apt-repository -y ppa:certbot/certbot && sudo apt-get update && sudo apt-get install -y certbot
```

## Using the `standalone` mode

```
sudo certbot certonly -d subdomain.example.com --standalone -m tech@healthians.com --agree-tos
```
