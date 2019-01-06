Maven-GitHub Release Demo
========

Overview
--------

This is a demo project used to show off various Maven deployment
fuctions versus GitHub.

Features of this project include:

* Maven is used to automatically roll/tag a source release in GitHub
* Maven is used upload compiled maven artifacts to a Maven artifact repo stored in a GitHub repo
	* The GitHub repo could be a separate repo from the source, or simply a dedicated branch in the same repo
	* Consumers can simply point at the GitHub repo as a Maven repo


Setting Up GitHub Credentials
--------

You add your GitHub credential to a `<server>` entry in 
your standard Maven settings file (`~/.m2/settings.xml`).

For example (where `github` is arbitrary, but matches our example `pom.xml`):

	<settings>
		<servers>
			<server>
				<id>github</id>
				<username>USERNAME</username>
				<password>PASSWORD</password>
			</server>
		</servers>
	</settings>

If you happen to use 2-factor authentication,
you can create a Personal Access Token in GitHub, then provide this
as a password (in which case you don't need a username):

<settings>
	<servers>
		<server>
			<id>github</id>
			<password>PERSONAL_ACCESS_TOKEN</password>
		</server>
	</servers>
</settings>


Setting Up Maven Central
--------

The `com.github.github:site-maven-plugin` that this project uses doesn't
appear to be available in the default Maven Central repo. Instead, I had
to add jCenter to my general-purpose Maven proxy server. You may need
to do this to your Maven proxy server, or you local `settings.xml` file.

Here's the repo I added (which is a superset of Maven Central):

	http://jcenter.bintray.com/


Tagging/Deploying a Release
--------

Here's how I generally tag/deploy the next release of the example Maven project:

	# Tag/push the next version
	mvn -B -DpreparationGoals=validate release:prepare

	# Build/deploy the tagged version
	mvn -B release:perform

	# Cleanup files used to track the release process locally
	mvn -B release:clean

Here's how I generally build/deploy an arbitrary (previously-tagged) release:

	git checkout release/1.2
	mvn clean deploy

See [Tag and Deploy Commands in Detail](Tag-and-Deploy-Commands-in-Detail.md)
for more information about these commands.


Accessing the Resulting Maven Artifacts
--------

Users of our deployed artifacts can get to them by adding this to their
list of Maven artifact repos:

	<repositories>
			<repository>
					<id>ryankenney_maven-github-release-demo</id>
					<url>https://raw.github.com/ryankenney/maven-github-release-demo.mvn/mvn-repo/</url>
			</repository>
	</repositories>

