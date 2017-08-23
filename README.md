# Gatekeeper
![Coverage Status](https://travis-ci.org/jamesabrown/gatekeeper.svg?branch=master) [![Coverage Status](https://coveralls.io/repos/github/jamesabrown/gatekeeper/badge.svg?branch=master)](https://coveralls.io/github/jamesabrown/gatekeeper?branch=master)

Gatekeeper is an API that allows dynamic management of an AWS security group. 

* You can have an IP added to the security group with the whitelist endpoint on the API. 
 To do so, create a POST request to `http://example.com/whitelist`. The body should be in
 in a JSON format configured like the following: `{"username": "user", "ip": "127.0.0.1"}`.
 All fields are required in order to complete the request. `username` is just used for 
 logging purposes. `ip` is the string that will be used to add to the security group. 
 
* Gatekeeper will run a method called expire which will remove any entries older than 48 hours.

* The HTTP_KEY header is required to match the GK_AUTH_TOKEN environment variable for authentication. 

## Configuration
Gatekeeper requires several environment variables in order to properly authenticate and function. These are defined in env.list.

These are the following environment variables. 

ENV Variable                    | Description
------------------------------- | ----------------------------------------------
AWS_PROFILE                     | AWS profile config(`default` is fine)
AWS_REGION                      | AWS region used in the AWS plugin
GK_SGID                         | AWS GroupID to be modified
AWS_ACCESS_KEY                  | AWS Access Key
AWS_SECRET_ACCESS_KEY           | AWS Secret Access Key
GK_AUTH_TOKEN                   | Used to validate the Http requests
EXPIRE_TIME                     | Expire time of IPs entered into the security group

* Copy env.list.sample to env.list and populate with correct values

### Optional parameters
If the environment variable `ALLOWED_COUNTRIES` is set, Gatekeeper will only allow countries defined within that variable.
`ALLOWED_COUNTRIES` is expected to have values arranged in the following way: `United States,Canada,Brazil`

## Run Gatekeeper
Gatekeeper has a Docker container built to run the app easily with the following commands

```shell
docker pull jamesabrown/gatekeeper
docker run -d -p 80:4567 --env-file ./env.list jamesabrown/gatekeeper
```

