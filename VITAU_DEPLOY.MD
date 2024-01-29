## Building
```cmd
$ ./bin/build
$ cp target/uberjar/metabase.jar bin/docker/metabase.jar
$ docker build -t $REPOSITORY_URI:latest bin/docker/
```

## Running local
```cmd
export MB_DB_TYPE=postgres
export DOCKER_DEFAULT_PLATFORM=linux/amd64

$ docker run -p 3000:3000 \
  -e "MB_DB_TYPE=postgres" \
  -e "MB_DB_CONNECTION_URI=postgres-test-db-uri" \
  -e "MB_SITE_URL=http://localhost:3000" \
  -e "MB_REDIRECT_ALL_REQUESTS_TO_HTTPS=false" \
  --name metabase $REPOSITORY_URI
```


## Requirements to deploy
### AWS CLI
https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html
AWS Credentials are stored in ~/.aws/credentials
### Docker
https://docs.docker.com/desktop/

## Deploy
```cmd
$ docker tag $REPOSITORY_URI:latest $REPOSITORY_URI:{increment_id}
$ docker push $REPOSITORY_URI:latest
```

## Update Version
Upload and Deploy Dockerrun.aws.json file to Elastic Beanstalk
```json
{
  "AWSEBDockerrunVersion" : "1",
  "Image" : {
    "Name" : "$REPOSITORY_URI:latest",
    "Update" : "true"
  },
  "Ports" : [ {
    "ContainerPort" : "3000"
  } ],
  "Logging" : "/var/log/metabase"
}
```
