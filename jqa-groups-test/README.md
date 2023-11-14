# Issues observed here

Two issues here:

## 1) Not recognizing groups configured in '.jqassistant.yml' of a maven module

### steps to recreate
* execute 'mvn clean install' on 'jqa-groups-test/jqa-groups-test-module/'
* execute 'mvn clean install' on 'jqa-groups-test/'
* compare build logs

### observed
* On 'jqa-groups-test-module/' analysis happens in 'jqassistant-maven-plugin:2.0.8:analyze':
  * [INFO] --- jqassistant-maven-plugin:2.0.8:analyze (default-cli) @ jqa-groups-test-module ---
  * [INFO] Rules directory 'C:\MyFiles\projects\jqa-groups-test\jqa-groups-test\jqa-groups-test-module\jqassistant' does not exist, skipping.
  * [INFO] Executing analysis for 'jqa-groups-test-module'.
  * ...

* On 'jqa-groups-test/' nothing happens in 'jqassistant-maven-plugin:2.0.8:analyze':
  * [INFO] --- jqassistant-maven-plugin:2.0.8:analyze (default-cli) @ jqa-groups-test-module ---
  * [INFO]
  * [INFO] --- maven-install-plugin:2.4:install (default-install) @ jqa-groups-test-module ---

### expected
No matter if the build is started from parent or module, the configured group in '.jqassistant.yml' of the 
module should be used for jqa analysis.

As you can see in issue 2) the '.jqassistant.yml' is definitely used in both cases but the configured  groups
seem to be ignored. 

## 2) Not recognizing '.jqassistant.yml' of a maven module

### steps to recreate
* remove 'configurationLocation' from 'jqa-groups-test/jqa-groups-test-module/.jqassistant.yml'
* execute 'mvn clean install' on 'jqa-groups-test/jqa-groups-test-module/'
* execute 'mvn clean install' on 'jqa-groups-test/'
* compare build logs

### observed
* On 'jqa-groups-test-module/' right before 'jqassistant-maven-plugin:2.0.8:analyze' 'src/main' is entered
  * '[INFO] Entering src/main'
* On 'jqa-groups-test/' right before 'jqassistant-maven-plugin:2.0.8:analyze' 'src/main' is not entered

I also observed other parameters not to be used from the yml so it seems the entire yml is not loaded at all. 

### expected
No matter if the build is started from parent or module, the '.jqassistant.yml' of the module should be used
during module build

