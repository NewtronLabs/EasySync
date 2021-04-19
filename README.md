# Easy Sync

The Easy Sync library allows you to turn asynchronous calls into synchronous ones with ease. 

----


## How to Use 

### Setup

Include the below dependencies in your `build.gradle` project.

```gradle
buildscript {
    repositories {
        google()
        jcenter()
        maven { url "https://newtronlabs.jfrog.io/artifactory/libs-release-local" }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.5.2'
        classpath 'com.newtronlabs.android:plugin:4.0.3'
    }
}

allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://newtronlabs.jfrog.io/artifactory/libs-release-local" }
    }
}

subprojects {
    apply plugin: 'com.newtronlabs.android'
}
```

In the `build.gradle` for your app.

```gradle
dependencies {
    compileOnly 'com.newtronlabs.easysync:easysync:4.0.0'
}
```

### EasySynch - Sample
To turn your asynchronous call simply wrap it in a ISynchroTask and pass and create a SynchExecutor to handle it's execution.

```java
SynchExecutor executor;
// Wrap your asynchronous call. In this same we use a thread for simplicity. 
executor = new SynchExecutor(new ISynchroTask()
{
     @Override
     public void execute()
     {
          new Thread(new Runnable()
          {
               @Override
               public void run()
               {
                    Log.d("EasySync", "Start of long operation.");
                    try
                    {
                        // The sleep is in place to simulate a long operation on this example.
                        Thread.sleep(3 * 1000);
                    }
                    catch(InterruptedException e)
                    {
                        e.printStackTrace();
                    }
                    Log.d("EasySync", "Long operation ended.");
                    
                    // Complete call.
                    asynchCallback();
               }
          }).start();
     }
});
        
// Execute your calls in sequence as normal.        
Log.d("EasySync", "Asynch Call Start");

// Run the asynchronous call.
executor.execute();
Log.d("EasySync", "Asynch Call Ended");

// Sample callback to illustrate call needed on completion of the asynchronous call only.
void asynchCallback()
{
    // On the callback of your asynchronous call complete 
    // to make sure to signal the executor of the completion.
    executor.complete();
}       

```

## License
https://gist.github.com/NewtronLabs/216f45db2339e0bc638e7c14a6af9cc8

## Contact

solutions@newtronlabs.com
