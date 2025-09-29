---
content_type: page
description: This section provides a detailed description of the Battlecode software,
  the installation process, configuration, and guidelines on getting started.
draft: false
learning_resource_types: []
ocw_type: CourseSection
parent_title: Specs and Software
parent_type: CourseSection
parent_uid: b953ba9d-34cd-a7ba-c5ff-0952e4179b9a
title: Battlecode 2013 Software
uid: 5c284dcb-09b7-b8e2-b704-cf0a29e6f23a
video_metadata:
  youtube_id: null
---
« {{% resource_link "b953ba9d-34cd-a7ba-c5ff-0952e4179b9a" "Return to Specs and Software" %}}

Instruction and information are provided on the following: [Overview](#overview), [System Requirements](#system-requirements), [Installation](#installation), [Running Matches](#running-matches), [Debugging your Player](#debugging-your-player), [Uploading your Player](#uploading-your-player), [Advanced Configuration](#advanced-configuration), [Appendix: Configuration Properties and Command-line Arguments](#appendix), [Scala](#scala)

## {{< anchor "overview" >}}{{< /anchor >}}Overview

The Battlecode software consists of three major components:

- **The player library / API**: These are the classes that you will import and build against when writing a player.
- **The server**: This is the software that computes Battlecode matches. For most users, the server will run transparently, so you don't have to worry about it. However, advanced server setups are possible, allowing you to compute matches on one machine and view them on another.
- **The client**: This is the software that displays Battlecode matches. For most users, the client will automatically create a server for running a match and display that match as it computes. The client also plays match files like those from scrimmage matches and the tournaments.

## {{< anchor "system-requirements" >}}{{< /anchor >}}System Requirements

The Battlecode software requires:

- [JDK 6.0](https://www.oracle.com/java/technologies/javase-java-archive-javase6-downloads.html) or higher

The following hardware is recommended for best performance:

- At least 512 MB of RAM

Ensure that you are using the correct version of Java. You are using Java 5 if you receive errors such as the following:

```plaintext
  [javac] class file has wrong version 50.0, should be 49.0
  [javac] Please remove or make sure it appears in the correct subdirectory of the classpath.
```

We recommend that you install either [Apache Ant](http://ant.apache.org/), [Eclipse](https://www.eclipse.org/), or [Netbeans](https://netbeans.org).

Battlecode will run on Java 7. Please be aware, however, that the scrimmage server runs Java 6, and it will not be able to compile your code if you use Java 7 specific language features.

## {{< anchor "installation" >}}{{< /anchor >}}Installation

### First Installation

To install the Battlecode software, download the installer and execute it. On Windows systems, this is usually accomplished by double-clicking the downloaded jar file. On other systems, you can run `java -jar battlecode-[version].jar` at a prompt. The installer will guide you through the software setup.

Since you'll probably be working in installation directory, you should install Battlecode to a user-writable location (i.e. `~/Battlecode` or in `/My Documents`, but not in `/usr/local`).

### Upgrading the Software

The installer is also capable of updating an existing Battlecode installation to a newer version. To perform an update, run the installer, choose the directory of the existing installation, and confirm that you'd like to overwrite the target directory.

Note that if you have an active Internet connection when running the Battlecode software, it will automatically notify you when a newer version of the software is available. The first time it detects a new version, a pop-up dialog appears when the software is started; each subsequent start shows an exclamation icon in the lower-left hand corner of the match dialog.

### Building a Player

The included `build.xml` file allows you to compile your player and prepare it for submission without having to worry about the Java classpath and other settings. To take advantage of this, simply place the source code for your player(s) in a subdirectory of `teams` folder in your Battlecode installation directory.

For instance, if you are team #1, you'd have a folder called `team001` in your `teams` folder, and in `team001` you'd have `RobotPlayer.java`, etc.

After you've placed your source code there, navigate to the installation directory and run:

```plaintext
ant build
```

This command will invoke the Java compiler on all source code in the `teams` folder.

### Using Apache Ant

Apache Ant is a Java-based build system similar in theory to UNIX `make`.

On every system you will need to set the JAVA\_HOME environment variable to point to the installation path of your JDK.

You may also need the ANT\_HOME environment variable in some cases. Just set this to be the path to your ant installation and you should be good to go.

You are not required to use the Ant build script, though it's been designed so that you can get up and running quickly and easily.

You can check which version of Java Ant is using with the command:

```plaintext
ant –diagnostics
```

### Using the Eclipse IDE

Update to Eclipse 3.5 or higher if you have not done so already. After you have installed the software, open Eclipse and choose "File" -> "New" -> "Project…" and choose "Java Project from Existing Ant Buildfile". Navigate to the build.xml in your install folder and use this one. Name your project, or leave it as the default and click 'Link to build file in the filesystem' and click 'Finish'. Alternatively, you can use "Java Project from Existing Source" and then in the import screen click the (+) next to teams.

When it finishes, Eclipse will add the "teams" folder as a source folder and add the included jar files to your build path. (If you installed Ant, you'll see a lots of files beginning with "ant-" on your build path. You can safely remove them from your build path, but don't delete them!) You can begin developing a player.

When you're ready to run a match, right click the `build.xml` file in your project root directory and choose "Run As" -> "Ant Build". You can re-run the match software by choosing the external tools runner in the Eclipse toolbar (the green-and-white run icon with a small bag beneath it).

## {{< anchor "running-matches" >}}{{< /anchor >}}Running Matches

### Local Matches

Local matches are the most common way to run a match—they are computed and rendered simultaneously on the same machine. To run a "local" match, use the ant run command from your installation directory. (If you're not using Ant, you can run the `battlecode.client.Main` class from the command line or an IDE.)

When running a local match, you also have the option of saving the match to a file so that you can play it back without having to recompute it. To do this, check the "Save match to file" box on the main dialog and choose the location of the file. Note that the file will be overwritten if it already exists.

### Remote Matches

It is also possible to compute and view matches on two separate machines via TCP / IP. Let's say you have two machines—A, which has the players you've written, will be the machine that computes the match; B will be the machine that views the match.

First, on machine A, use `ant serve` to run a match server. Then, on machine B, use `ant run` to run the main dialog. Choose "Connect to remote match server" on the dialog and enter the host or IP address of machine A. When you've entered the address, the dialog on machine B will ask machine A for the teams and maps it knows about, so you can choose them from the dropdown list.

### Playing Back from a File

If you have a match file that you'd like to play back (i.e., from saving one previously) you can choose "Play back from match file" and choose the match file to play back. The remainder of the dialog settings will be ignored.

### Match Parameters

A dialog box will appear that allows you to choose the teams and maps to run. The teams and maps that appear in the dropdown boxes are those that the software knows about at runtime. If the team or map you're trying to run does not appear in the dropdown box, it isn't on your classpath or map path.

### Match Sets

This year, each match between two teams consists of a set of games. To run multiple games in a match, use the add and remove buttons below the map dropdown box to add maps to the list. A game will be played on each map in the list, in order. If you don't add any maps, the match will consist of one game, on the map selected in the dropdown box.

## {{< anchor "debugging-your-player" >}}{{< /anchor >}}Debugging your Player

Normally, the software computes the match well ahead of what is being currently viewed. However, selecting "compute and view match synchronously" from the match dialog puts the software in "lockstep mode", where the computation and viewing move in lockstep. This is generally slower than running them independently, but it allows for some interesting debugging features.

While in lockstep mode, right-clicking on an open square in the map brings up a menu that lets you add new units to the map. Right-clicking on an existing unit allows you to set its control bits, which the robot's player can query and react to. You can also drag-and-drop units on the map.

These debugging features are intended to help you test your robots in situations that might otherwise be hard to get them into (e.g., what happens if one of my archons gets cut off from the rest…?). However, if the players are not written defensively, these unexpected manual changes can interfere with their control logic. Keep this in mind when using the debugging features.

Also, during the tournament and scrimmages, you will not be able to manually affect the game in this way.

## {{< anchor "uploading-your-player" >}}{{< /anchor >}}Uploading your Player

You should upload a zip or jar file containing your team's source code. You should use the package name `teamXXX` where XXX is your team number. You can build this jar automatically using the command.

```plaintext
ant -Dteam=teamXXX jar
```

Alternatively, creating a zip file of the `teamXXX` directory should work.

## {{< anchor "advanced-configuration" >}}{{< /anchor >}}Advanced Configuration

The Battlecode distribution includes a configuration file, `bc.conf`, which allows you to tweak some of the software's settings. Appendix A contains a listing of configurable properties.

The properties can also be set in any way that Java properties are set. This includes using "-D\[property\]=\[value\]" on the `java` or `ant` command line.

## {{< anchor "appendix" >}}{{< /anchor >}}Appendix: Configuration Properties and Command-line Arguments

### Computation Throttling

When running a local match, the game engine attempts to periodically delay to prevent starving the match viewer of CPU time. Altering the following two settings may yield better local match performance:

- `bc.server.throttle`: determines how to delay match computation; can be set to either "yield" or "sleep".
- `bc.server.throttle-count`: the number of rounds between sleep / yield.

### Engine Settings

The following settings can be used to enable or disable certain aspects of the engine.

- `bc.engine.debug-methods`: "true" or "false"; whether or not the engine will run debug methods.
- `bc.engine.silence-a` and `bc.engine.silence-b`: "true" or "false"; whether or not the engine will suppress printouts for the appropriate team.
- `bc.engine.gc`: "true" or "false"; whether or not to periodically force garbage collection in the game engine—this option causes decreased performance but may help if the virtual machine runs out of memory during computation.
- `bc.engine.gc-rounds`: how many rounds between forced invocation of the garbage collector; the default is 50.
- `bc.engine.upkeep`: "true" or "false"; if "false", the engine will not charge units their energon upkeep each round.
- `bc.engine.breakpoints`: "true" or "false"; if "false" the engine will skip breakpoints in player code.

### "Headless" Mode Settings

"Headless" mode runs matches without displaying them in a client. This mode can be run via "ant file" in the installation, or with a "-h" argument when calling the Main class directly.

### Client Settings

- `bc.game.renderprefs2d`: preferences for the 2d client. For example, if you wanted to turn off the rendering of broadcasts and gridlines, you could set this property to "bg". See the "Shortcut Keys" section for a complete listing of toggles.

### Miscellaneous Settings

- `bc.game.map-path`: the folder in which to look for map files.
- `bc.dialog.skip`: "true" or "false"; whether or not to show the setup dialog when the software is started. If "true" the parameters most recently entered into the dialog will be used.

### Command-line Arguments

- `-c (config file name)`: use the specified config file when running the match; defaults to `bc.conf` in ant.
- `-h`: use "headless" mode (see above); equivalent to "ant file".
- `-s`: use "server" mode—listen for a connection from a remote client and send match data via TCP; equivalent to "ant serve".
- `-n`: skip the match dialog; equivalent to `bc.dialog.skip=true` config file option (see above).

### Client Settings

- `bc.client.renderprefs2d`: A list of toggles to set in the 2D client. (See Shortcut Keys below.) For example, if you want to turn off broadcasts, grid lines, and transfers, you would set `bc.client.renderprefs2d=bgt`.
- `bc.client.sound-on`: "true" or "false"; whether or not to play sound effects.

### Shortcut Keys

{{< tableopen >}}{{< theadopen >}}{{< tropen >}}{{< thopen >}}
KEYS
{{< thclose >}}{{< thopen >}}
EFFECTS
{{< thclose >}}{{< trclose >}}{{< theadclose >}}{{< tbodyopen >}}{{< tropen >}}{{< tdopen >}}
A
{{< tdclose >}}{{< tdopen >}}
Toggle unit sensor and attack ranges
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
B
{{< tdclose >}}{{< tdopen >}}
Toggle unit broadcasts
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
D
{{< tdclose >}}{{< tdopen >}}
Toggle discrete movement mode
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
E
{{< tdclose >}}{{< tdopen >}}
Toggle HP bars
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
F
{{< tdclose >}}{{< tdopen >}}
Toggle fast forward
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
G
{{< tdclose >}}{{< tdopen >}}
Toggle grid lines
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
I
{{< tdclose >}}{{< tdopen >}}
Toggle action lines
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
J
{{< tdclose >}}{{< tdopen >}}
Toggle slow-motion
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
K
{{< tdclose >}}{{< tdopen >}}
Toggle attack circles
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
L
{{< tdclose >}}{{< tdopen >}}
Toggle flux bars
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
R
{{< tdclose >}}{{< tdopen >}}
Toggle regeneration
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
S
{{< tdclose >}}{{< tdopen >}}
Skip 100 rounds
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
T
{{< tdclose >}}{{< tdopen >}}
Toggle transfers
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
X
{{< tdclose >}}{{< tdopen >}}
Toggle unit explosions
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
/
{{< tdclose >}}{{< tdopen >}}
Find unit by ID
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}

\<

Pause

{{< tdclose >}}{{< tdopen >}}
 
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
\\>
{{< tdclose >}}{{< tdopen >}}
Pause
{{< tdclose >}}{{< trclose >}}{{< tropen >}}{{< tdopen >}}
Escape
{{< tdclose >}}{{< tdopen >}}
Quit
{{< tdclose >}}{{< trclose >}}{{< tbodyclose >}}{{< tableclose >}}

## {{< anchor "scala" >}}{{< /anchor >}}Scala

Most contestants choose to write their players in Java, but we also support Scala (or a mix of Java and Scala). If you want to use Scala, you should download the [Scala compiler (JAR - 10.8MB)](/ans7870/6/6.370/iap13/scala-compiler-2.9.1.jar), [Scala library (JAR - 8.4MB)](/ans7870/6/6.370/iap13/scala-library-2.9.1.jar), and {{% resource_link "883bf3b8-7adf-861e-e3de-addcd0dabb1e" "jline (JAR)" %}}, and place them in your `lib` directory. You can also download a simple {{% resource_link "1116c4ea-d4e9-1191-965f-f489ee13f411" "example player (SCALA)" %}} written in Scala.

« {{% resource_link "b953ba9d-34cd-a7ba-c5ff-0952e4179b9a" "Return to Specs and Software" %}}