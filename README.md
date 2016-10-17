# react-native-couchbase-lite

* [Using rnpm](#using-rnpm)
* [Install manually](#install-manually)
* [Usage](#usage)
* [Examples](#examples)

Couchbase Lite binding for react-native on both iOS and Android. It works by exposing some functionality to the native Couchcase Lite and the remaining actions are peformed via the REST API.

## Using rnpm

Create a new React Native project:
```
react-native init UntitledApp
cd UntitledApp
```
Install the React Native Couchbase Lite module:
```
npm install --save react-native-couchbase-lite
```
Link the module using rnpm:
```
rnpm link react-native-couchbase-lite
```

Follow the steps below to finish the installation.

### iOS

* Download the Couchbase Lite iOS SDK from [here](http://www.couchbase.com/nosql-databases/downloads#) and drag CouchbaseLite.framework, CouchbaseLiteListener.framework in the Xcode project:

![](http://cl.ly/image/3Z1b0n0W0i3w/sdk.png)

### Android

Add the following in `android/app/build.gradle` under the `android` section:
```
packagingOptions {
		exclude 'META-INF/ASL2.0'
		exclude 'META-INF/LICENSE'
		exclude 'META-INF/NOTICE'
}
```

## Install manually

### iOS (react-native init)

```
$ react-init RNTestProj
$ cd RNTestProj
$ npm install react-native-couchbase-lite --save
```

* Drag the ReactCBLite Xcode project in your React Native Xcode project:

![](http://cl.ly/image/0S133n1O3g3W/static-library.png)

* Add ReactCBLite.a (from Workspace location) to the required Libraries and Frameworks.

![](http://cl.ly/image/2c0Z2u0S0r1G/link.png)

* From the `Link Binary With Libraries` section in the `Build Phases` of the top-level project, add the following frameworks in your Xcode project (Couchbase Lite dependencies):

	- libsqlite3.0.tbd
	- libz.tbd
	- Security.framework
	- CFNetwork.framework
	- SystemConfiguration.framework

* Download the Couchbase Lite iOS SDK from [here](http://www.couchbase.com/nosql-databases/downloads#) and drag CouchbaseLite.framework, CouchbaseLiteListener.framework in the Xcode project:

![](http://cl.ly/image/3Z1b0n0W0i3w/sdk.png)

### iOS (Cocoapods)

* Install both npm modules:

```
$ npm install react-native
$ npm install react-native-couchbase-lite
```

* In the `Podfile`, add dependencies:

```
pod 'React', :path => './node_modules/react-native'
pod 'ReactNativeCouchbaseLite', :path => './node_modules/react-native-couchbase-lite'
```

* Install the Cocoapods dependencies:

```
$ pod install
```

* So far so good! Next, you must install CBLRegisterJSViewCompiler.h and libCBLJSViewCompiler.a. You can download both components from [here](http://www.couchbase.com/nosql-databases/downloads#). Drag CBLRegisterJSViewCompiler.h into the couchbase-lite-ios Pod:

![](http://cl.ly/1L2s28462D2W/Image%202016-01-26%20at%2012.47.12%20pm.png)

* Add the libCBLJSViewCompiler.a static library:

![](http://cl.ly/2G1L392h0b1Z/Image%202016-01-27%20at%2010.30.32%20pm.png)

### Android

* Add the `react-native-couchbase-lite` project to your existing React Native project in `android/settings.gradle`

	```
	...
	include ':react-native-couchbase-lite'
	project(':react-native-couchbase-lite').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-couchbase-lite/android')
	```

* Add the Couchbase Maven repository to `android/build.gradle`

	```
	allprojects {
			repositories {
					mavenLocal()
					jcenter()
	
					// add couchbase url
					maven {
							url "http://files.couchbase.com/maven2/"
					}
			}
	}
	```

* Add `android/app/build.gradle`

	```
	apply plugin: 'com.android.application'
	
	android {
			...
	
			packagingOptions {
					exclude 'META-INF/ASL2.0'
					exclude 'META-INF/LICENSE'
					exclude 'META-INF/NOTICE'
			}
	}
	
	dependencies {
			compile fileTree(dir: 'libs', include: ['*.jar'])
			compile 'com.android.support:appcompat-v7:23.0.0'
			compile 'com.facebook.react:react-native:0.12.+'
	
			// Add this line:
			compile project(':react-native-couchbase-lite')
	}
	```

* Register the module in `getPackages` of `MainActivity.java`

  ```
  @Override
  protected List<ReactPackage> getPackages() {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
          new ReactCBLiteManager()				<----- Register the module
      );
  }
  ```

## Usage

In your app entry, init and start the Couchbase Lite Listener

```js
import {
  rncblite,
  ReactCBLite,
} from 'react-native-couchbase-lite';

rncblite(manager => {
      // use manager to perform operations
    });
```

A username/password pair is automatically generated to protect access to the database endpoint.

## Examples

The full api is derived from the [Couchbase Lite Swagger spec](http://developer.couchbase.com/mobile/swagger/couchbase-lite/).

Use the `help()` method to print API methods and parameters to the console.

```javascript
manager.help();                 // prints all tags and endpoints
manager.database.help();        // prints all endpoints for the database tag
manager.database.put_db.help(); // prints the list of parameters for PUT /{db}
```

See the [ReactNativeCouchbaseLiteExample](https://github.com/couchbaselabs/react-native-couchbase-lite/tree/master/ReactNativeCouchbaseLiteExample) for a few examples.

#### LICENSE

MIT
