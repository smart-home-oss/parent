Integrate travis with bintray:

Test locally
In Maven’s `setting.xml file`, add the following section to declare your Bintray credentials. Use your API key as your password (not your login password):

    <server>
        <id>bintray-smart-home-oss-maven</id>
        <username>rodislav</username>
        <password>**********</password>
    </server>

- https://maven.apache.org/guides/mini/guide-encryption.html

Add the the following Distribution Management section to your project’s pom.xml file to tell Maven to deploy into this package using the credentials you configured in the previous step:

    <distributionManagement>
        <repository>
            <id>bintray-smart-home-oss-maven</id>
            <name>smart-home-oss-maven</name>
            <url>https://api.bintray.com/maven/smart-home-oss/maven/parent/;publish=1</url>
        </repository>
    </distributionManagement>

Login
- https://docs.github.com/en/github/authenticating-to-github/creating-a-personal-access-token#creating-a-token
- https://docs.travis-ci.com/user/github-oauth-scopes/
    - Repositories on https://travis-ci **.COM** (Private and public) #
    - https://docs.travis-ci.com/user/github-oauth-scopes/#travis-ci-for-private-projects
- token scopes `read:org, repo, user:email, write:repo_hook`
- `travis login --github-token xxxxxxxxxx`
- if it fails try to re-sync https://travis-ci.org/account/preferences


`Failed to execute goal org.apache.maven.plugins:maven-release-plugin:3.0.0-M1:prepare (default-cli) on project parent: An error is occurred in the checkin process: Exception while executing SCM command. Detecting the current branch failed: fatal: ref HEAD is not a symbolic ref`
- https://git-scm.com/docs/git-config#Documentation/git-config.txt-coresymlinks

Encrypt key
- https://docs.travis-ci.com/user/deployment/bintray/

Maven release 
- token scopse `public_repo`

https://synyx.de/blog/using-travis-ci-to-deploy-to-maven-repositories-and-github-releases/
https://blog.ham1.co.uk/2018/02/13/maven-release-plugin-for-github/

    <server>
        <id>github</id>
        <username>rodislav</username>
        <password>{xxxxxx}</password>
    </server>

- https://stackoverflow.com/questions/28282572/maven-release-plugin-git-credentials
- <project.scm.id>github</project.scm.id>

Integrate travis with sonarcloud:
- [SonarQube Scanner for Maven ](https://docs.travis-ci.com/user/sonarcloud/#sonarqube-scanner-for-maven)
- [travis :: encryption keys](https://docs.travis-ci.com/user/encryption-keys/)
- [CI/CD springboot applications using travis](https://sivalabs.in/2018/01/ci-cd-springboot-applications-using-travis-ci/)
- [Getting started with spring boot travis and heroku](https://medium.com/@felippepuhle/getting-started-with-spring-boot-travis-and-heroku-4562a723fd0e)

We need to run the travis command from within the repository folder. 
We'll assume our repository folder is called `house-manager`

```shell script
cd /path/to/house-manager
travis encrypt e3125a3...f156 --pro
```

> `--pro` because we use _travis_.__com__ 

Output will be like
```shell script
Please add the following to your .travis.yml file:

  secure: "MifJv.....dyP31btE="
```

Then we need to put the output, the text from double quotes, in our `travis.yml`

```shell script
...

addons:
  sonarcloud:
    organization: "smart-home-oss"
    token:
      secure: "MifJv.....dyP31btE=" # <------- HERE

...
```