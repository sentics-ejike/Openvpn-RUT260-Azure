# Install OpenVPN on Ubuntu
# As always, first make sure that your system has up-to-date packages.

`sudo apt update`
`sudo apt upgrade`
# Next, install required dependencies.

`sudo apt install ca-certificates wget net-tools gnupg`
# Add the OpenVPN server to your repository list.
`sudo wget -qO - https://as-repository.openvpn.net/as-repo-public.gpg | sudo apt-key add -`
#OK

`echo "deb http://as-repository.openvpn.net/as/debian focal main" | sudo tee /etc/apt/sources.list.d/openvpn-as-repo.list`

# Now, you can proceed with updating the package lists and installing OpenVPN Access Server using the following commands:
`sudo apt update`
`sudo apt install openvpn-as`
########################################################
#FOR DEPENDENCY ISSUE, 
#ADD focal-security repository to resolve a specific dependency issue,
`echo "deb http://security.ubuntu.com/ubuntu focal-security main" | sudo tee /etc/apt/sources.list.d/focal-security.list`

`sudo apt-get update`
`sudo apt-get install libssl1.1`

# Then delete the focal-security list file you just created:
`sudo rm /etc/apt/sources.list.d/focal-security.list`
`sudo apt update`
`sudo apt install openvpn-as`

#REMOVE OPENVPN 
`sudo apt remove openvpn-as`
`sudo rm -rf /usr/local/openvpn_as`





Access Server Web UIs are available here:
Admin  UI: https://10.100.3.4:943/admin
Client UI: https://10.100.3.4:943/

https://74.234.28.22:943/admin
To login please use the "openvpn" account with "s0WJyCQhid4T" password.
(password can be changed on Admin UI)

https://74.234.28.22:943/?src=connect
or user:sentics password:ProServ01!


https://74.234.28.22:943/?src=connect
user:rut360 password:sentics

# Access the Admin Dashboard

# Mapp this private ip to use public ip, go to vm on azure portal, under network settings and create port rule to allow 943
https://20.107.170.193:943/admin/

# Admin UI using the provided URL (e.g., https://10.100.3.4:943/admin) 
# and the default credentials ("openvpn" as the username and "sqxd2BhHO2xD" as the password)

# Install the easy-rsa package
`sudo apt install openvpn easy-rsa`

# Set up the Certificate Authority
# first copy the easy-rsa directory to /etc/openvpn
`sudo make-cadir /etc/openvpn/easy-rsa`

# navigate to the /etc/openvpn/easy-rsa directory and start setting up your Certificate Authority (CA) and generating the necessary certificates and keys for your 
# OpenVPN server.
`sudo su`
`cd /etc/openvpn/easy-rsa`

`./easyrsa init-pki`
`./easyrsa build-ca`

# Create server keys and certificates
`./easyrsa gen-req myservername nopass`

# Diffie Hellman parameters must be generated for the OpenVPN server. pki/dh.pem:
`./easyrsa gen-dh`

# And finally, create a certificate for the server:

`./easyrsa sign-req server myservername`

# All certificates and keys have been generated in subdirectories. Common practice is to copy them to /etc/openvpn/:

`cp pki/dh.pem pki/ca.crt pki/issued/myservername.crt pki/private/myservername.key /etc/openvpn/`

# Create client certificates

`./easyrsa gen-req myclient1 nopass`
`./easyrsa sign-req client myclient1`


# Import the file
`easyrsa import-req /incoming/myclient1.req myclient1`
`sign-eq`

#  copy the following files to the client

`pki/ca.crt`
`pki/issued/myclient1.crt`

