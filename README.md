# basic-cdk

Starting point for my Serverless CDK projects

# Setup

## Install the AWS CDK CLI

```
npm i -g aws-cdk
```

## Set up a profile

https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-profiles.html

# To deploy

Create `cdk/config.ts` and put something like this in it:

```
export default {
  environment: "Dev",
  projectname: "ProjectName"
};
```

Then...

```
mkdir build
cd cdk
yarn build && cdk synth
cdk deploy ProjectNameDevDynamoDb --require-approval never
cdk deploy ProjectNameDevCognito --require-approval never &> ../cdkdeployresult_cognito.txt
wait $!
cd ..
echo "export default {};" > src/config.js
node parseAwsOutputs.js cdkdeployresult_cognito.txt src/config.js
yarn build
cd cdk
cdk deploy ProjectNameDevS3 --require-approval never
```
