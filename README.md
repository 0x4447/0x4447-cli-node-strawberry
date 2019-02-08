# 🍓 Strawberry

This CLI was create out of frustration after we struggled countless times to do a simple HTTPS redirect, for example from: https://example.com to https://www.example.com.

This is a straightforward action over a non-encrypted connection. However, trying to make it work over HTTPS is an entirely different story, since you need a server with a certificate that can establish a secure connection before the server can tells the browser where to go.

It's simple to do this with a regular server, but more complicated if working with serverless technologies, not to mention the certificate setup and management for such a simple task.

That's how Strawberry came to be. We actually use serverless technology to fight serverless technologies through the use of AWS S3 and AWS CloudFormation to handle this simple operation. Go figure...

Strawberry automatically creates the entire stack for you. All you have to do is provide some basic information, and leave the rest up to the CLI.

# How to install

```
] sudo npm install -g @0x4447/strawberry
```

# How to use

```
] strawberry -s source.example.com -d destination.example.com
```

# Where to get help

```
] strawberry -h
```

# What to expect

### This CLI...

- Creates an S3 bucket with redirect enabled to the destination domain
- Creates a certificate for the source domain
- Creates a CloudFront distribution with the new certificate
- Configures Route 53 so the source domain points to CloudFront

**WARNING**: What if the certificate takes too long to validate? After 60 seconds, the app quits and prints out a detailed explanation of what your next steps should be. Take the time to thoroughly go over the printout, and you'll be good.

### High-level flow looks like this:

- User visit the source domain
- A secure connection is established
- CloudFront reads the S3 bucket configuration
- The user is redirected to the destination domain

All of this is done through the use of SSL.

# Credentials

To use this CLI, create a programmatic user or create a role with the following permissions:

- AmazonS3FullAccess
- CloudFrontFullAccess
- AmazonRoute53FullAccess
- AWSCertificateManagerFullAccess

# Is deployment instant?

No, it's not. The following aspects don't happen right away:

- SSL Certificate confirmation
- CloudFront distribution

### SSL Certificate confirmation

The timeframe for this process ranges from 10 seconds to 24 hours. It's completely unpredictable, and there's no way to speed up the process. Because of this, the app quits if the certificate isn't confirmed within 60 seconds. When that happens, go to the AWS Console to monitor the certificate.

### CloudFront distribution

This takes up to 15 or 20 minutes, but when you reach this point, you can be certain that the configuration is correct. At this point, you just need to wait until the process is complete. Only then will the domain deliver the website.

# The end

If you enjoyed this project, please consider giving it a 🌟. And check out our [0x4447 GitHub account](https://github.com/0x4447), which contains additional resources you might find useful or interesting.

## Sponsor 🎊

This project is brought to you by 0x4447 LLC, a software company specializing in building custom solutions on top of AWS. Follow this link to learn more: https://0x4447.com. Alternatively, send an email to [hello@0x4447.email](mailto:hello@0x4447.email?Subject=Hello%20From%20Repo&Body=Hi%2C%0A%0AMy%20name%20is%20NAME%2C%20and%20I%27d%20like%20to%20get%20in%20touch%20with%20someone%20at%200x4447.%0A%0AI%27d%20like%20to%20discuss%20the%20following%20topics%3A%0A%0A-%20LIST_OF_TOPICS_TO_DISCUSS%0A%0ASome%20useful%20information%3A%0A%0A-%20My%20full%20name%20is%3A%20FIRST_NAME%20LAST_NAME%0A-%20My%20time%20zone%20is%3A%20TIME_ZONE%0A-%20My%20working%20hours%20are%20from%3A%20TIME%20till%20TIME%0A-%20My%20company%20name%20is%3A%20COMPANY%20NAME%0A-%20My%20company%20website%20is%3A%20https%3A%2F%2F%0A%0ABest%20regards.).
