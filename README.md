# Package Repository


## Deployment instructions

There are many ways to deploy your packages to the package registry. Two different methods of how to do that will be described below.

- [Deployment method 1](#deployment-method-1)
- [Deployment method 2](#deployment-method-2)


## Deployment method #1

### Generate ***personal token***

- Go to your [account -> Preferences -> Access Tokens](https://gitlab.anorbank.uz/-/profile/personal_access_tokens)
- Create ***Token name***
> ***Token name*** might be anything
- Check ***api*** option
- Click ***Create personal access token*** button
- Save ***Your new personal access token*** somewhere

### Create settings.xml

Create ***settings.xml*** file in your project's root directory.
File's content described below

```xml
<settings xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://maven.apache.org/SETTINGS/1.0.0"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 https://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>gitlab-maven</id>
            <configuration>
                <httpHeaders>
                    <property>
                        <name>Private-Token</name>
                        <value>YOUR_PRIVATE_TOKEN</value>
                    </property>
                </httpHeaders>
            </configuration>
        </server>
    </servers>
    <activeProfiles/>
</settings>
```
> ***YOUR_PRIVATE_TOKEN*** is the token, which has been created by you at previous step

### Add the following content to your pom.xml

```xml
<repositories>
	<repository>
		<id>gitlab-maven</id>
		<url>https://gitlab.anorbank.uz/api/v4/projects/PROJECT_ID/packages/maven</url>
	</repository>
</repositories>
<distributionManagement>
	<repository>
		<id>gitlab-maven</id>
		<url>https://gitlab.anorbank.uz/api/v4/projects/PROJECT_ID/packages/maven</url>
	</repository>
	<snapshotRepository>
		<id>gitlab-maven</id>
		<url>https://gitlab.anorbank.uz/api/v4/projects/PROJECT_ID/packages/maven</url>
	</snapshotRepository>
</distributionManagement>
```

### Deploy

Open command line and run ***mvn clean deploy -s settings.xml***. Or in Intellij Idea open ***Maven*** tab and double-click on ***Lifecycle -> clean/deploy***
> ***PROJECT_ID*** is the id of current **Package Repository** project. By now it is ***243*** (Replace ***PROJECT_ID*** to ***243***)

## Deployment method #2

### Generate ***the token***

See [Generate personal token](#1-generate-personal-token)

### Create ***settings.xml***

Create ***settings.xml*** file in maven's .m2 root directory (for windows it is ***c:\Users\USER_NAME\.m2***)
File's content described below

```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
                      http://maven.apache.org/xsd/settings-1.0.0.xsd">
	<activeProfiles>
		<activeProfile>gitlab</activeProfile>
	</activeProfiles>
	<profiles>
		<profile>
			<id>gitlab</id>
			<properties>
				<altDeploymentRepository>gitlab::default::https://gitlab.anorbank.uz/api/v4/projects/PROJECT_ID/packages/maven</altDeploymentRepository>
			</properties>
			<repositories>
				<repository>
					<id>central</id>
					<url>https://repo1.maven.org/maven2</url>
					<releases>
						<enabled>true</enabled>
					</releases>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</repository>
				<repository>
					<id>gitlab</id>
					<url>https://gitlab.anorbank.uz/api/v4/projects/PROJECT_ID/packages/maven</url>
					<snapshots>
						<enabled>true</enabled>
					</snapshots>
				</repository>
			</repositories>
		</profile>
	</profiles>
	<servers>
		<server>
			<id>gitlab</id>
			<configuration>
				<httpHeaders>
					<property>
						<name>Private-Token</name>
						<value>YOUR_PRIVATE_TOKEN</value>
					</property>
				</httpHeaders>
			</configuration>
		</server>
	</servers>
</settings>
```

### Deploy

Open command line and run ***mvn clean deploy***

> If you use the second method of package deployment, dependency declaration in pom.xml of another project will be enough

> ***PROJECT_ID*** is the id of current **Package Repository** project. By now it is ***243*** (Replace ***PROJECT_ID*** to ***243***)
