Wildcard S3 DNS
=============

Wildcard (S3) DNS, enables any domain to instantly use S3. It can get DNS records off of S3 buckets. This DNS server has a build in http server to redirect base domain to WWW. Anyone can add S3DNS-US-EAST-1-1.DIGIHAVEN.COM to their domain registry and create a bucket after their domain name ( www.example.com ).

Steps to making this work
=============

1) Register domain name (for example coolwebsite.com) and use nameservers:
```````
S3DNS-US-EAST-1-1.DIGIHAVEN.COM 
S3DNS-US-EAST-1-2.DIGIHAVEN.COM
````````

(optional) additional servers:
````````
S3DNS-US-EAST-1-3.DIGIHAVEN.COM
S3DNS-US-EAST-1-4.DIGIHAVEN.COM
S3DNS-US-EAST-1-5.DIGIHAVEN.COM
S3DNS-US-EAST-1-6.DIGIHAVEN.COM
````````

2) Create bucket www.coolwebsite.com

Thats all! No more complicated DNS settings needed!

How it works
=============

Bind dones not support whildcard zones (it does support whild card A records...)
This dns server can programmably serve entire whildcard zones.
It can server out DNS records to CNAMES automaticly making S3 usage easy by using existing installs at in your whois records (Domain reg)

What the free/public DNS does
=============

The public DNS is located at S3DNS-US-EAST-1-*.DIGIHAVEN.COM
 
Forwards any domain (example.com to www.example.com) (save $10/month/ec2.micro?) for people who can't use “zone apex” aka "root cname" (godaddy DNS, BIND, etc....)
	example.com A record -> HTTP 301 Moved Permanently www.example.com

DNS whildecard records to S3 (save $0.50/zone/month/amazon)
	www.example.com cname -> www.scamwiki.org.s3-website-us-east-1.amazonaws.com


What the free/public testing DNS does
=============

Hosts any records (Such as A, MX, AAAA, TEXT, etc...) that must be in the root of the bucket (dns.record).

dns.record example:
```````````````
$TTL 3D
@       IN      SOA     #DNS#	admin.example.com. (
                        199609206       ; serial, todays date + todays serial #
                        8H              ; refresh, seconds
                        2H              ; retry, seconds
                        4W              ; expire, seconds
                        1D )            ; minimum, seconds

@	A		#HTTPS# ; 	will override the www forwarder and use the HTTPS bucket server
www	CNAME		 diffrentbucket.example.com.s3-website-us-east-1.amazonaws.com ; 	will override the www cname

	IN	MX	1  	ASPMX.L.GOOGLE.COM.
	IN	MX	5 	ALT1.ASPMX.L.GOOGLE.COM.
	IN	MX	5	ALT2.ASPMX.L.GOOGLE.COM.
	IN	MX	10	ASPMX2.GOOGLEMAIL.COM.
	IN	MX	10	ASPMX3.GOOGLEMAIL.COM.
	IN	MX	10	ASPMX4.GOOGLEMAIL.COM.
	IN	MX	10	ASPMX5.GOOGLEMAIL.COM.
```````````````

Hosts HTTPS and certs located in your buckets (MUST BE DIGIHAVE USER READABLE AND NO PUBLIC!). DNS certs must be encrypted using RSA (2048-bit+) so only DIGIHAVEN can read it.

Example of (in root of bucket):
```````
Key: ssl.key
Cert: ssl.cert
Bundel: ssl.ca (optional but suggested!)
```````

The (planed) public testing key that must be used:
```````
-----BEGIN PUBLIC KEY-----
MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvBGuAGOGg8fpVB3X82jb
yaBboHA4y4N94BboBVbOJM5X9dcKnZFx5hG7vKCITiSxB3HOIswMX1YuCZnJaeAd
hcGzO93yjD/JKG4+6oOmCgsR5egVyHm+kVZxdpiM8FjfQ02zC60BtTCtS5b9DYbg
dwOqPdnJvIaOooq+GrPXk5YP6UW2mTxO6fV+tlgsvlT014bH2+7mktIejJvI5M2S
vMCjj7KbbmvYXNY++u27FJywfZDvhkxsDA+wrHGlWpgoNzFxQ006rruoPfINcmK4
VWSSPpOvnhL73abABulsRuE5W5/Ubfe3ttijl6WUBNGLgTEBJL813mVzXKhD1PNN
mQIDAQAB
-----END PUBLIC KEY-----
```````

Note: There will be a program to encrypt and upload the certs to your bucket.
Note: THE TEST SERVER IS NOT >>YET<< PCI COMPLIANT!
