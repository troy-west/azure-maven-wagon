Maven Wagon Provider for Windows Azure Blob storage
---------

Provides a Maven Wagon implementation (see [Maven website](http://maven.apache.org/wagon/index.html)) for [Windows Azure Blob storage](http://www.windowsazure.com/en-us/documentation/services/storage/). You can use Blob storage containers to store the artifacts of a remote/distribution repository of Maven.

Configuration
----
1) Add an extension tag to your pom.xml file to enable the usage of Windows Azure Blob storage Wagon provider implementation

		<extensions>
			<extension>
				<groupId>com.microsoft.windowsazure</groupId>
				<artifactId>maven-wagon-azure-blob</artifactId>
				<version>1.0.2-SNAPSHOT</version>
			</extension>
		</extensions>
	</build>
	
2) Add a plugin repository entry in your pom.xml file to be able to download the extension

	<pluginRepositories>
		<pluginRepository>
			<id>ExtensionsRepo</id>
			<url>http://jmamavenrepos.blob.core.windows.net/releases</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</pluginRepository>
	</pluginRepositories>	
	
2) Set up distribution management in your pom.xml file

	<distributionManagement>
		<snapshotRepository>
			<id>AzureTest</id>
			<name>AzureTest</name>
			<url>azureblob://DefaultEndpointsProtocol=https;AccountName=mavenwagontests/tests</url>
		</snapshotRepository>
	</distributionManagement>

Note: 'url' tag starts with 'azureblob' which is the wagon protocol for Windows Azure Blob storage. 
After 'azureblob://' a valid Windows Azure Storage connection string gets specified, followed by '/' and the name of the blob container to be used.

3) Add Windows Azure Storage credentials to Maven's settings.xml file

		<server>
			<id>AzureTest</id>
			<privateKey>[YourWindowsAzureStorageAccountKey]</privateKey>
		</server>	
	</servers>

The 'privateKey' tag contains the Windows Azure Storage account key

Leiningen
----

To use from Leiningen add the following to your ~/.lein/profile.clj

``` clojure
:your-profile {:deploy-repositories {"snapshots" {:url "azureblob://<<YourAzureStorageAccountConnectionString>>" :no-auth true}
                                     "releases" {:url "azureblob://<<YourAzureStorageAccountConnectionString>>" :no-auth true}}
               :plugins [[com.microsoft.windowsazure/maven-wagon-azure-blob "1.0.2-SNAPSHOT"]]}
```

Then you can deploy with:

``` shell
lein with-profile your-profile deploy
```
