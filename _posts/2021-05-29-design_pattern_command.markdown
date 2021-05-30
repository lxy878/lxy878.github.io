---
layout: post
title:      "Design Pattern: Command"
date:       2021-05-29 04:40:22 -0400
description: 'ommand Design Pattern is a behavioral design pattern to encapsulate a request as an object, thereby letting you parameterize clients...'
permalink:  design_pattern_command
---

Command Design Pattern is a behavioral design pattern to encapsulate a request as an object, thereby letting you parameterize clients with different requests, queue or log requests, and support undoable operations.

Benefits of this pattern:
* allows you to set aside a list of commands for later use.
* a class is a great place to store the procedure you want to be executed.
* you can store multiple commands in a class to use over and over.
* you can implement undo procedures for past commands.

For example, we attempt to create a file system for both Unix OS and Windows OS.

Before applying the command design pattern, we need two file system classes that contain actions under both OS.

Therefore, create an interface for both file systems.

```
public interface FileSystem {
    void openFile();
    void writeFile();
    void closeFile();
}
```

Then, create Unix file system and Windows file system classes inherited with FileSystem interface.

```
public class WindowsFileSystem implements FileSystem{

    @Override
    public void openFile() {
        System.out.println("Windows OS: open a file");
    }

    @Override
    public void writeFile() {
        System.out.println("Windows OS: write a file");
    }

    @Override
    public void closeFile() {
        System.out.println("Windows OS: close a file");
    }
}
```

```
public class UnixFileSystem implements FileSystem {

    @Override
    public void openFile() {
        System.out.println("Unix OS: open a file");
    }

    @Override
    public void writeFile() {
        System.out.println("Unix OS: write a file");
    }

    @Override
    public void closeFile() {
        System.out.println("Unix OS: close a file");
    }
}
```
Now, we can use the command pattern.

First, create an interface for commands.

```
public interface Command {
    void execute();
}
```

Then, create classes for openFile(), writeFile() and closeFile().

```
public class OpenCommand implements Command {
    private FileSystem fileSystem;

    public OpenCommand(FileSystem fileSystem){
        this.fileSystem = fileSystem;
    }

    @Override
    public void execute() {
        fileSystem.openFile();
    }
}
```

```
public class WriteCommand implements Command {
    private FileSystem fileSystem;

    public WriteCommand(FileSystem fileSystem){
        this.fileSystem = fileSystem;
    }

    @Override
    public void execute() {
        fileSystem.writeFile();
    }
}

```

```
public class CloseCommand implements Command {
    private FileSystem fileSystem;

    public CloseCommand(FileSystem fileSystem){
        this.fileSystem = fileSystem;
    }

    @Override
    public void execute() {
        fileSystem.closeFile();
    }
}
```

Last, create a processor class to execute created commands in orders.

```
public class CommandProcessor {
    private Queue<Command> commandQueue = new LinkedList<>();

    public void addCommand(Command command){
        commandQueue.add(command);
    }

    public void executeCommands(){
        while (commandQueue.size()>0){
            Command command = commandQueue.poll();
            command.execute();
        }
    }
}
```

Now, test out.

```
   public static void main(String[] args){
        CommandProcessor commandProcessor = new CommandProcessor();

        FileSystem unix = new UnixFileSystem();
        commandProcessor.addCommand(new OpenCommand(unix));
        commandProcessor.addCommand(new WriteCommand(unix));
        commandProcessor.addCommand(new CloseCommand(unix));

        FileSystem windows = new WindowsFileSystem();
        commandProcessor.addCommand(new OpenCommand(windows));
        commandProcessor.addCommand(new WriteCommand(windows));
        commandProcessor.addCommand(new CloseCommand(windows));

        commandProcessor.executeCommands();
    }
```

```
// OUTPUTS:
Unix OS: open a file
Unix OS: write a file
Unix OS: close a file
Windows OS: open a file
Windows OS: write a file
Windows OS: close a file
```

To sum up, the command design pattern allows decoupling classes that invoke operations from classes that perform these operations. Also, it allows us to create new commands into the app without breaking existing client code. This pattern, however, requires us to create many small classes that store lists of commands.