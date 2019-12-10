# Bitbucket Cloud build status notifier plugin for Jenkins - [![Build Status][jenkins-status]][jenkins-builds]

This Jenkins plugin is simply a fork of original [Bitbucket Cloud build status notifier plugin for Jenkins](https://github.com/jenkinsci/bitbucket-build-status-notifier-plugin) at version [1.4.1](https://github.com/jenkinsci/bitbucket-build-status-notifier-plugin/releases/tag/1.4.1)

Please refer to documentation of original plugin for additional information


## Features (of Fork)

The original plugin does not have the option to provide custom build URL while updating build status in Bitbucket
This might not always be a problem unless you want custom portal or code analysis tool to be destination of your build status depending on the type of failure

The fork has simply added buildURL as extra parameter, which falls back to original functionality if the buildUrl is not provided


## Dependencies
This plugin depends on other Jenkins plugins, please refer to documentation of original plugin


## Instructions

### Create a OAuth Consumer
First you need to get a OAuth consumer key/secret from Bitbucket.

1. Login into your Bitbucket account.
2. Click your account name and then in **Settings** from the menu bar.
3. Click **OAuth** from the menu bar.
4. Press the **Add consumer** button.
6. The system requests the following information:
 1. Give a representative **name** to the consumer e.g. Jenkins build status notifier.
 2. Although is not used, a **Callback URL** must be set e.g. ci.your-domain.com.
 2. Leave blank the **URL** field.
 3. Add **Read** and **Write** permissions to **Repositories**.
 4. Click **Save** button and a **Key** and **Secret** will be automatically generated.

### Ensure Jenkins URL is set
Second, ensure that Jenkins URL is properly set:

1. Open Jenkins **Manage Jenkins** page.
2. Click **Configure System** page.
3. Got to the section **Jenkins Location**.
4. Set correct URL to **Jenkins URL**.
5. Click **Save** button.

### Add OAuth Credentials to Jenkins
Third, you need to add the Bitbucket OAuth Consumer credentials. You have two ways to configure it globally or locally:

#### Global

1. Open Jenkins **Manage Jenkins** page.
2. Click **Configure System**.
3. Go to the section **Bitbucket Build Status Notifier plugin**
4. If you still don't have stored the credentials click **Add**, otherwise you can skip this step.
 1. Select **Username with password**.
 2. Set the the OAuth consumer **key** in **Username**.
 3. Set the the OAuth consumer **secret** in **Password**.
 4. Click **Add** button.
5. Select the desired credentials.
6. Click **Save** button.

#### Local

1. Go to the Job you want notifies the builds to Bitbucket.
2. Click **Configure**.
3. Click **Add post-build action**.
4. Select **Bitbucket notify build status**.
5. Click **Advanced** button.
6. If you still don't have stored the credentials click **Add**, otherwise you can skip this step.
 1. Select **Username with password**.
 2. Set the the OAuth consumer **key** in **Username**.
 3. Set the the OAuth consumer **secret** in **Password**.
 4. Click **Add** button.
7. Select the desired credentials.

### Pipeline step to notify Bitbucket

Once you have configured the credential, you can notify BitBucket from your Pipeline script through the `bitbucketStatusNotify` step.

#### Usage

The `bitbucketStatusNotify` step notifies the status of a build identified by a build key and build name to BitBucket.
If `buildKey` and `buildName` parameters are not provided, a standard name will be assigned to the build (NameOfYourJob #numberOfBuild - eg. MyProject #32).

```groovy
  ...  
    try {
        myTestFunction()
        bitbucketStatusNotify(
          buildState: 'SUCCESSFUL',
          buildKey: 'test',
          buildName: 'Test',
          buildUrl: "${MyNonCIURL}", //Or this could simply be "http://www.google.com"
          repoSlug: 'my-awesome-project',
          commitId: 'a83c709e9d514421ef614ef0a1117366c84c6304'
        )
    } catch(Exception e) {
        bitbucketStatusNotify(
          buildState: 'FAILED',
          buildKey: 'test',
          buildName: 'Test',
          buildUrl: "${MyNonCIURL}", //Or this could simply be "http://www.google.com"
          buildDescription: 'Something went wrong with tests!',
          repoSlug: 'my-awesome-project',
          commitId: 'a83c709e9d514421ef614ef0a1117366c84c6304'
        )
    }
  ...
```

#### API Summary

Parameter:

| Name | Type | Optional | Description |
| --- | --- | --- | --- |
| `buildState` | `INPROGRESS`,`SUCCESSFUL`,`FAILED` | no | The status of the current build phase
| `buildKey` | String | yes | The unique key identifying the current build phase
| `buildName` | String | yes | The build phase's name shown on BitBucket
| `buildUrl` | String | yes | The Url of non CI build system, do not include this parameter if you want the plugin to use default CI build Url   
| `buildDescription` | String | yes | The build phase's description shown on BitBucket
| `repoSlug`| String | yes | The slug of the bitbucket repository to send the notification to
| `commitId` | String | yes | The id of the commit to attach the status notification to 

Note that the `repoSlug` and `commitId` parameters work only when they are both specified.

## Contributions

This repository will be unlikely to be upgraded to next version, please use original repo for Contributions :)

## License

The MIT License (MIT)

Copyright (c) 2015 Kedar Kulkarni

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated
documentation files (the "Software"), to deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit
persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of
the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE
WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR
OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

