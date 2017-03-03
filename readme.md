[![Build Status](https://travis-ci.org/ert78gb/google-datastore-emulator.svg?branch=master)](https://travis-ci.org/ert78gb/google-datastore-emulator)

Google Cloud Datastore Emulator
===============================

This package helps to start / stop [Google Datatstore Emulator](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/datastore/) with javascript.
Perfect to support unit testing where the persistent layer is the gcloud Datastore.

The wrapper sets DATASTORE_EMULATOR_HOST and DATASTORE_PROJECT_ID environment variables.

#Prerequisites
To use the emulator you need to install [Google Cloud SDK](https://cloud.google.com/sdk/downloads)

#Installation
```
npm install google-datastore-emulator --save-dev
```

#Usage
I think the package is the most suitable for unit testing.
 
```javascript
const datastore = require('@google-cloud/datastore');
const Emulator = require('google-datastore-emulator');

describe('test suit', ()=>{
    process.env.GCLOUD_PROJECT = 'project-id'; // Set the datastore project Id globally

    let emulator;
    
    before(()=>{
        const options = {
            legacy:true // if you need legacy support
        };
        
        emulator = new Emulator(options);
        
        return emulator.start();
    });
    
    after(()=>{
        return emulator.stop();
    });
    
    it('test case', ()=>{
        // your test
    });
})

```

## Options

parameter (type) | default value | description
----------|---------------|-------------------
projectId (string) | empty | This variable is datastore project Id. If it is empty, GCLOUD_PROJECT environment variable will be used. Either you should set it directly or the environment variable should be set.
storeOnDisk (boolean) | false | The datastore either persists the entities on disk or not.
dataDir (string) | empty | The emulator creates a directory where the project files are stored. If it is empty the emulator default value will be used. You could set relative ./directory or absolute path /tmp/dir1/dir2/. If this directory does not exist, it will be created.
clean (boolean) | true | If dataDir value is set and 'clean' value is true then the package deletes the dataDir. The package **does not** delete the gcloud emulator default directory. 
host (string) | empty | If it is empty the'localhost' of google default value is used. It can take the form of a single address (hostname, IPv4, or IPv6)
port (number) | empty | If it is empty the emulator selects a random free port.
debug (boolean) | false | If it is true, it writes the console.logs of the emulator onto the main process console.
legacy (boolean) | false | If it is true, it supports Cloud Datastore API v1beta2.

## Methods

name | description
-----|------------
start | Starts the emulator and returns a Promise.
stop | Stops the emulator and returns a Promise.

#License

MIT
