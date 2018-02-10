Please take a few minutes and let us know how we are doing by filling out this survey: https://goo.gl/forms/97TYeG6Wx5CA4lBV2

---

# Easy Sync

The Easy Sync library allows you to turn asynchronous calls into synchronous ones with ease. 

----


## How to Use 

### Setup

Include the below dependencies in your `build.gradle` project.

```gradle
buildscript {
    repositories {
        jcenter()
        maven { url "http://code.newtronlabs.com:8081/artifactory/libs-release-local" }
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:2.3.3'
        classpath "com.newtronlabs.android:plugin:1.2.0"
    }
}

allprojects {
    repositories {
        jcenter()
        maven { url "http://code.newtronlabs.com:8081/artifactory/libs-release-local" }
    }
}

subprojects {
    apply plugin: 'com.newtronlabs.android'
}
```

In the `build.gradle` for your app.

```gradle
dependencies {
    provided 'com.newtronlabs.easysync:easysync:2.0.0'
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

Easy Sync binaries and source code can only be used in accordance with Freeware license. That is, freeware may be used without payment, but may not be modified. The developer of Easy Sync retains all rights to change, alter, adapt, and/or distribute the software. Easy Sync is not liable for any damages and/or losses incurred during the use of Easy Sync.

You may not decompile, reverse engineer, pull apart, or otherwise attempt to dissect the source code, algorithm, technique or other information from the binary code of Easy Sync unless it is authorized by existing applicable law and only to the extent authorized by such law. In the event that such a law applies, user may only attempt the foregoing if: (1) user has contacted Newtron Labs to request such information and Newtron Labs has failed to respond in a reasonable time, or (2) reverse engineering is strictly necessary to obtain such information and Newtron Labs has failed to reply. Any information obtained by user from Newtron Labs may be used only in accordance to the terms agreed upon by Newtron Labs and in adherence to Newtron Labs confidentiality policy. Such information supplied by Newtron Labs and received by user shall not be disclosed to a third party or used to create a software substantially similar to the technique or expression of the Newtron Labs Easy Sync software.

You are solely responsible for determining the appropriateness of using Easy Sync and assume any risks associated with Your use of Easy Sync. In no event and under no legal theory, whether in tort (including negligence), contract, or otherwise, unless required by applicable law (such as deliberate and grossly negligent acts) or agreed to in writing, shall Newtron Labs be liable to You for damages, including any direct, indirect, special, incidental, or consequential damages of any character arising as a result of this License or out of the use or inability to use the Easy Sync (including but not limited to damages for loss of goodwill, work stoppage, computer failure or malfunction, or any and all other commercial damages or losses), even if Newtron Labs has been advised of the possibility of such damages. 

## Contact

contact@newtronlabs.com
