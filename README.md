# samlapi
This is a fork of https://github.com/CU-CloudCollab/samlapi with the changes required to get it to work with my fresh install of Shibboleth. 

The config for that is in another github project - https://github.com/jasonumiker/shibboleth3-aws-duo-config

This project uses a Selenium-ised Firefox to fill in the fields and click the buttons of your Shibboleth page and then grab the SAMLResponse and use that to ask the AWS STS service for the temporary credentials. 

I'd like to thank sbower and Cornell for a great starting point for my efforts.

## Usage
1. Edit the bin/samlapi.rb file to put the address of your Shibboleth login page into it
1. Run the build.sh file to build the samlapi container with that config locally or run
```
docker build -t samlapi .
```
1. Run the samlapi script to run the container or run
```
docker run -it --rm -v ~/.aws:/root/.aws samlapi
```

After this command has been run it will prompt you for your username and password.  This will be used to login you into your Shibboleth. You will get a push from DUO.  Once you have confirmed the DUO notification, you will be prompted to select the role you wish to use for login, if you have only one role it will choose that automatically.  The credentials will be placed in the default credential file (~/.aws/credentials) and can be used as follows:

```
aws --profile saml s3 ls
```

## Troubleshooting
To troubleshoot this run the container interactively with `docker run -it --rm --entrypoint=/bin/bash samlapi` and then use irb to paste the samlapi.rb file line-by-line in to the Ruby interpreter and see the results.

Note that you need to use `password = STDIN.gets.chomp` instead when running the command interactively. Paste everything before the username capture, run the username then the password capture, then paste continue - otherwise it'll treat the next line of code as a username/password.

## More Info
https://github.com/CU-CloudCollab/samlapi

http://blogs.cornell.edu/cloudification/2016/07/05/using-shibboleth-for-aws-api-and-cli-access/

Base Image can be found here: https://github.com/CU-CommunityApps/docker-xvfb-firefox.
