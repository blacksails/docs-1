

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Migrate To Rig
This document provides guidance on how to migrate existing users to Rig from firebase. 
Rig provides a general framework for migrating users from other systems aswell - see the [general migration documentation](../migration/custom). 

## Users Migration
Migrating users from firebase can easily be done using the Rig CLI. The migration can be performed in two different ways:
1. Provide rig with a firebase credentials file, and let rig authenticate with firebase and migrate all users automatically.
2. Download a users.json file from firebase, and provide this to rig, which will then migrate all users in the file. This is useful, if you want greater control over what users are migrated, or if you do not want rig to authenticate with firebase.

To migrate users from firebase, you can use the 
```bash
rig user migrate
```
command. This is an interactive command, which will ask you for what platform you wish to migrate from, what method you wish to use, and then the required information for the chosen method.

### Prerequisites
In order to migrate users from firebase, you naturally need to have a firebase account with some users. You will also need to find the hashing key used by firebase. This can be found in the firebase console under Authentication, and then the "Password Hash Parameters" option, under the three dots menu. If you want to migrate using the credentials, you will also need a firebase credentials file, which can be obtained by following the [firebase documentation](https://firebase.google.com/docs/admin/setup#initialize-sdk). If you want to migrate using the users.json file, you will need to download this from firebase. This can be done by using the [firebase CLI](https://firebase.google.com/docs/cli#install_the_firebase_cli), and then export the users using the command 
```bash
firebase auth:export users.json --format=json --project <project_id>
```

### Migration Using Credentials
After the initial command of `rig user migrate`, select `Firebase` as the platform you wish to migrate from. Then select `Credentials` as the method. You will then be asked for the `credentials file path`. After providing this, you will be asked to input the `Hashing Key`. The migration will then start, and you will be notified when it is done, with the amount of users migrated. 

### Migration Using users file
This is the same as the previous method, except you select `Users file` as the method, and then provide the path to the users.json file. You will then similarly be asked for the Hashing key, and the migration will then commence.