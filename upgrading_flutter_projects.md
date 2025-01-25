## Flutter: Upgrading old projects

### Basic Approach

- Ensure the Flutter development environment is up to date.
- Create a new Flutter project in a new folder using the original's names.
- Migrate the old project's assets, code, permissions & keys, etc. to the new project.
- Deal with remaining issues.

### Tasks

---

#### Gather 4 strings from the original project:

1. Project folder name
2. Project Name
    - _`name`_ property in _`pubspec.yaml`_
    - Often the same as the _project's (root) folder name_
      - If _project folder name_ != _`name`_ then I would use the  
        _`name`_ as Project Name to create the project,  
      and maybe rename the _project folder_ afterwards.
3. Project Description
    - _`description`_ property in _`pubspec.yaml`_
4. Project Organization  
    - See the _`organisation`_, _`applicationId`_,   
    _`namespace`_ and _`package`_ properties in  
    - _`/android/app/build.gradle`_,  
    - _`MainActivity.*`_, and  
    - the 3 _`AndroidManifeast.xml`_ files.  
    - Has the form: TLD.OrgDomain.ProjectName  
      - ie. [_com.acme-corp.proj_name_]()    

---

#### Upgrade Flutter

Use the flutter script command: _`flutter upgrade`_

---

#### Create new Flutter Project using Android Studio

- Use the `New Flutter Project` button in Android Studio
- Set project's name as per above
- Set project's description
- Set Organization
    

- ![NewFlutter project settings](upgrading_flutter_projects.png)

---

#### XXXX Change the new project's Name to match the original project XXXX

Not necessary in this migration method as it creates a new Flutter app  
using the original project's _`name`_, _`description`_ & _`organization`_

---

#### Migrate the app

Migrate _`pubspec.yaml`_, dependencies, etc.  
Migrate _`assets`_, _`icons`_, _`fonts`_ and _`lib`_ folders, etc.  
Migrate _`README.md`_, etc.

---

#### Migrate permissions

- Migrate app permissions across the various _`AndroidManifest.xml`_ files  
- Migrate API keys

---

#### If key(s) exist:

- Migrate the _`key.properties`_ file
- Manage the _`key.jks`_ file
- Migrate _`keystoreProperties`_ in _`/android/app/build.gradle`_

  - Insert the _`keystoreProperties`_ above the _`android`_ block:

  ```gradle
      def keystoreProperties = new Properties()
      def keystorePropertiesFile = rootProject.file('key.properties')
      if (keystorePropertiesFile.exists()) {
        keystoreProperties.load(new FileInputStream(keystorePropertiesFile))
      }

      android {
        ...
      }
  ```

- Migrate the _`signingConfigs`_ block in _`/android/app/build.gradle`_
  - Insert a _`signingConfigs`_ block above the _`buildTypes`_ block:
  ```gradle
      signingConfigs {
        release {
          keyAlias keystoreProperties['keyAlias']
          keyPassword keystoreProperties['keyPassword']
          storeFile keystoreProperties['storeFile'] ?
              file(keystoreProperties['storeFile']) :
              null
          storePassword keystoreProperties['storePassword']
        }
      }
  ```

---

#### Compile and Debug

Some things I needed to compile an upgraded project:

- Set the Project SDK and the Platform SDKs to 1.8 (Eclipse Foundation)

- Upgrade _`com.android.application`_ to 8.2.2 in _`/android/settings.gradle`_

If issues persist, the _[flutter_migrate](https://pub.dev/packages/flutter_migrate)_ tool may be useful.

Good luck!

---

##### References:

Alien Code, (13 Aug 2024). _Easy migration of old Flutter project..._  
[https://aliencode52.blogspot.com/2024/04/easy-migration-of-old-flutter-project.html](https://aliencode52.blogspot.com/2024/04/easy-migration-of-old-flutter-project.html)

Gupta, A. (2024). _Migrating old Flutter project to the latest version._ [https://dev.to/ankurg132/migrating-old-flutter-project-to-the-latest-version-1d06](https://dev.to/ankurg132/migrating-old-flutter-project-to-the-latest-version-1d06)

van den Eijnde, T. (31 Oct 2024). _How to Change the Name of Your Flutter Application_ [https://onlyflutter.com/how-to-change-the-name-of-your-flutter-application/](https://onlyflutter.com/how-to-change-the-name-of-your-flutter-application/)

---

#### Last update

_PS Gray, 26 Jan 2025_

---

Comments/Suggestions welcome...
