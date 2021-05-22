---
layout: post
title:      "Design Pattern: Strategy"
date:       2021-05-14 04:40:22 -0400
description: 'Strategy Design Pattern is a way to encapsulate a group of algorithms that can be interchangeable for user....'
permalink:  design_pattern_strategy
---

Strategy Design Pattern is a way to encapsulate a group of algorithms that can be interchangeable for users' usages.

An original class may have those problems:

* it is big and hard to maintain when the code base has more and more algorithms by the time.
* users want to use the right algorithm at an appropriate moment. 
* It's difficult to add new algorithms and vary existing ones when the code is a part of the program.

We can avoid these problems by strategy patterns, defining classes that encapsulate different algorithms.

For example, we are building a chat application. In the application, there are three ways to allow users to send messages by text, video, and audio.

First, create an interface for Message.

```
public interface Message {
    void sendMessage(String content);
}
```

Second, create three classes that inherited the Message interface.

```
public class VideoMessage implements Message {

    @Override
    public void sendMessage(String content) {
        System.out.println(content+" sent");
    }
}
```

```
public class TextMessage implements Message {

    @Override
    public void sendMessage(String content) {
        System.out.println(content+" sent");
    }
}

```

```
public class AudioMessage implements Message{

    @Override
    public void sendMessage(String content) {
        System.out.println(content+" sent");
    }
}
```

Finally, create a class for the chat window.

```
public class ChatWindow {
    public void sendMessage(Message message, String content){
        message.sendMessage(content);
    }
}
```


Now, test it out.
```
 public static void main(String[] args) {
        ChatWindow chatWindow = new ChatWindow();
        chatWindow.sendMessage(new VideoMessage(), "Video message");
        chatWindow.sendMessage(new AudioMessage(),"Audio message");
        chatWindow.sendMessage(new TextMessage(), "Text message");
 }
```

To sum up, strategy design pattern allows to swap algorithms used inside an object and to isolate the implementation details of an algorithm from the code that uses it. Even though the strategy design pattern has a very similar structure to the state design pattern, behaviors in the strategy pattern can be changed by the clients, and ones in the state pattern can be changed automatically by internal states.

