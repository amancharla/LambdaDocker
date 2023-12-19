#LambdaDocker

mkdir myproject && cd myproject

pyvenv venv

git init
echo 'venv' > .gitignore
activate venv/bin/activate

docker build --platform linux/amd64 -t docker-image:test .

docker run --platform linux/amd64 -p 9000:8080 docker-image:test

curl "http://localhost:9000/2015-03-31/functions/function/invocations" -d '{"payload":"hello world!"}'

docker ps

docker kill 3766c4ab331c


aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin ZZZ.dkr.ecr.us-west-2.amazonaws.com

aws ecr create-repository --repository-name hello-world --region us-west-2 --image-scanning-configuration scanOnPush=true --image-tag-mutability MUTABLE

docker tag docker-image:test ZZZ.dkr.ecr.us-east-1.amazonaws.com/hello-world:latest


docker push ZZZ.dkr.ecr.us-west-2.amazonaws.com/hello-world:latest

1. Create an execution role for the function, if you don't already have one. You need the Amazon Resource Name (ARN) of the role in the next step.


aws lambda create-function \
  --function-name hello-world \
  --package-type Image \
  --code ImageUri=ZZZ.dkr.ecr.us-west-2.amazonaws.com/hello-world:latest \
  --role arn:aws:iam::ZZZ:role/lambda-ex

aws lambda invoke --function-name hello-world response.json
