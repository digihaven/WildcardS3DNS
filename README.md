Wildcard S3 DNS
=============

Wildcard (S3) DNS, enables any domain to instantly use S3. It can get DNS records off of S3 buckets. This DNS server has a build in http server to redirect base domain to WWW. Anyone can add S3DNS-US-EAST-1-1.DIGIHAVEN.COM to their domain registry and create a bucket after their domain name ( www.example.com ).

Steps to making this work
=============

1) Register domain name (for example coolwebsite.com) and use:

Primary: S3DNS-US-EAST-1-1.DIGIHAVEN.COM
secondary: S3DNS-US-EAST-1-2.DIGIHAVEN.COM

Extra server:
S3DNS-US-EAST-1-3.DIGIHAVEN.COM
S3DNS-US-EAST-1-4.DIGIHAVEN.COM
S3DNS-US-EAST-1-5.DIGIHAVEN.COM
S3DNS-US-EAST-1-6.DIGIHAVEN.COM

2) create bucket www.coolwebsite.com

Thats all! No more complicated DNS settings needed!

How it works
=============

Bind dones not support whildcard zones (it does support whild card A records...)
This dns server can programmably serve entire whildcard zones.
It can server out DNS records to CNAMES automaticly making S3 usage easy by using existing installs at in your whois records (Domain reg)

What the public S3DNS-US-EAST-1-*.DIGIHAVEN.COM DNS does
=============

Forward example.com to www.example.com (save $10/month/ec2.micro?) for people who can't use root cname (godaddy DNS, BIND, etc....)
	example.com cname -> 301 Moved Permanently www.example.com

DNS whildecard records to S3 (save $0.50/zone/month/amazon)
	www.example.com cname -> www.scamwiki.org.s3-website-us-east-1.amazonaws.com
