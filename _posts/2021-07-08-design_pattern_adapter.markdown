---
layout: post
title:      "Structural Design Pattern: Adapter"
date:       2021-07-08 04:40:22 -0400
description: `The adapter design pattern can convert the interface of a class into another interface clients expect. This pattern lets classes work together that couldn't otherwise...`
permalink:  design_pattern_adapter
---

The adapter design pattern can convert the interface of a class into another interface clients expect. This pattern lets classes work together that couldn't otherwise because of incompatible interfaces.  The Adapter class allows the use of the available interface and the Target interface.  Any class can work together as long as the Adapter solves the issue that all classes must implement every method defined by the shared interface.

We use the Adapter pattern when
• use an existing class, and its interface does not match the one you need.
• create a reusable class that cooperates with unrelated or unforeseen classes, that is, classes that don't necessarily have compatible interfaces.
• use several existing subclasses, but it's un- practical to adapt their interface by subclassing every one. An object adapter can adapt the interface of its parent class.

For example, we have a program that plays mp3 and mp4. However, the program uses two sub-programs as a media player for mp3 and a media package for mp4. We improve the program that can use the media player to access mp4 too.

First, create two interfaces as MediaPackage and MediaPlayer.

```
public interface MediaPackage {
    void playFile();
}
```

```
public interface MediaPlayer {
    void play();
}
```

Then, create two classes for mp3 and mp4, implementing MediaPlayer and MediaPackage.

```
public class Mp3 implements MediaPlayer {
    private String title;
    public Mp3(String title){
        this.title = title;
    }

    @Override
    public void play() {
        System.out.println("Playing "+this.title);
    }
}
```

```
public class Mp4 implements MediaPackage {
    private String title;

    public Mp4(String title){
        this.title = title;
    }

    @Override
    public void playFile() {
        System.out.println("Playing "+this.title);
    }
}
```

Finally, create a class as the adapter for mp3.

```
public class Mp3Adapter implements MediaPlayer{
    private MediaPackage mediaPackage;

    public Mp3Adapter(MediaPackage mediaPackage){
        this.mediaPackage = mediaPackage;
    }

    @Override
    public void play() {
        mediaPackage.playFile();
    }
}
```

Now, test out.

```
public static void adapterTest(){
    MediaPackage mp4 = new Mp4("Sleeping Music");
    MediaPlayer mp3 = new Mp3("Relaxing Music");
    System.out.println("Media player only plays Mp3 and Media package only plays Mp4");
    mp4.playFile();
    mp3.play();
    System.out.println("using Mp3 adapter to play Mp4");
    MediaPlayer mp3Adapter = new Mp3Adapter(mp4);
    mp3Adapter.play();
}
```
```
// Outputs:
Media player only plays Mp3 and Media package only plays Mp4
Playing Sleeping Music
Playing Relaxing Music
using Mp3 adapter to play Mp4
Playing Sleeping Music
```

To sum up, the adapter design pattern allows separating the interface or data conversion code from the primary business logic of the program. Also, we can use this pattern to introduce new types of adapters into the program without breaking the existing client code, as long as they work with the adapters through the client interface.