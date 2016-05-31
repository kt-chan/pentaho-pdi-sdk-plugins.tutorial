A good way to debug PDI plugins is to deploy the plugin, launch Spoon, and connect the debugger to the Spoon JVM. This section explains how to debug a plugin in Eclipse.

1. Prepare Spoon for debugging.
  1. Start the Spoon JVM, allowing debug sessions and passing these arguments to the Spoon JVM.

          -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=1044
    The address argument can be any free port on your machine. This example uses port 1044. 
     
    If you are using `Spoon.bat` or `spoon.sh` to launch Spoon, create a copy of the file and edit it to include the debugging parameters to the Java options near the bottom of the file. If you are using a Mac app, add the JVM parameters to VMOptions key of `Data Integration 64-bit.app/Contents/Info.plist` or `Data Integration 32-bit.app/Contents/Info.plist` respectively. 

    When you start Spoon, debuggers connect on port 1044.
2. Launch a debug session.
  1. Ensure that Spoon is set up for debugging and running with the plugin deployed.
  2. Connect the Eclipse debugger by creating a debug configuration for your plugin project. From the **Run/Debug Configurations** menu, create a new configuration for **Remote Java Application**.
  3. Select your project, making sure the port matches the port configured in step 1.
  4. Decide whether you want to be able to kill the Spoon JVM from the debugger, then click **Apply** and **Debug**.

    The debugger opens, stops at the breakpoints you set, and in-line editing of the plugin source is enabled.