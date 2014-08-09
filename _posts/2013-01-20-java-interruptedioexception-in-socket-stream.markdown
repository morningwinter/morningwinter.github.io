---
layout: post
title:  "Java InterruptedIOException in Socket Stream"
date:   2013-01-20 23:03:52
categories: General
---

In java 6, the ObjectOutputStream over socket behaves differently on interruption among platforms. Here is the simple Server-Client test code using socket communication. Client sends 10 string messages to Server and interrupts the thread at the forth communication. This is a very simple socket communication java code.

Server source code:
{% highlight java %}
import java.io.*;
import java.net.*;

public class Server
{
    public static void main(String[] args)
    {
    ServerSocket server = null;
    Socket client = null;
    ObjectOutputStream out = null;
    ObjectInputStream in = null;
    try
    {
        System.out.println("Listening...");
        server = new ServerSocket(4321);
        client = server.accept();
        in = new ObjectInputStream(client.getInputStream());
        out = new ObjectOutputStream(client.getOutputStream());
        out.flush();

        while (true)
        {
        try
        {
            System.out.println("-- Receive  --");
            System.out.println(in.readObject());
            System.out.println();

            Thread.sleep(5*1000);

            out.writeObject("From Server: Hi.");
            out.flush();
        }
        catch(EOFException e)
        {
            e.printStackTrace();
            break;
        }
        }
    }
    catch(Exception e)
    {
        e.printStackTrace();
    }
    finally
    {
        try
        {
        if (out != null)
        {
            out.close();
        }

        if (server != null)
        {
            server.close();
        }

        if (client != null)
        {
            client.close();
        }
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }
    }
}
{% endhighlight %}

{% highlight java %}
import java.io.*;
import java.net.*;

public class Client
{
    public static void main(String[] args)
    {
    Socket client = null;
    ObjectOutputStream out = null;
    ObjectInputStream in = null;
    try
    {
        client = new Socket("localhost", 4321);
        out = new ObjectOutputStream(client.getOutputStream());
        out.flush();
        in = new ObjectInputStream(client.getInputStream());

        System.out.println("Buffer size: " + client.getSendBufferSize());

        for (int i = 0 ; i < 10 ; i++)
        { 

        if ( i == 3 )
        {
            Thread.currentThread().interrupt();
            System.out.println("Interrupted.");
        }
        out.writeObject("From Client: Hellow." + i);
        out.flush();
        System.out.println(in.readObject());
        }
        
    }
    catch(Exception e)
    {
        e.printStackTrace();
    }
    finally
    {
        try
        {
        if (out != null)
        {
            out.close();
        }

        if (in != null)
        {
            in.close();
        }

        if (client != null)
        {
            client.close();
        }
        }
        catch (Exception e)
        {
            e.printStackTrace();
        }
    }
    }
}
{% endhighlight %}

<strong>Linux</strong>
The output in Linux. The socket IO in linux is using blocking IO. The blocking IO won't be blocked until the socket buffet is full. It won't throw an interruptedIOException when the java thread is interrupted in the test code.

{% highlight text %}
Buffer size: 25434
From Server: Hi.
From Server: Hi.
From Server: Hi.
Interrupted.
From Server: Hi.
From Server: Hi.
From Server: Hi.
From Server: Hi.
From Server: Hi.
From Server: Hi.
From Server: Hi.
{% endhighlight %}

<strong>Solaris</strong>
The output in Solaris. As we can see, it throw an interruptedIOException when the java thread is interrupted, which is different from the result above.
{% highlight text %}
Buffer size: 49152
From Server: Hi.
From Server: Hi.
From Server: Hi.
java.io.InterruptedIOException
    at java.net.SocketOutputStream.socketWrite0(Native Method)
    at java.net.SocketOutputStream.socketWrite(SocketOutputStream.java:92)
    at java.net.SocketOutputStream.write(SocketOutputStream.java:136)
    at java.io.ObjectOutputStream$BlockDataOutputStream.drain(ObjectOutputStream.java:1847)
    at java.io.ObjectOutputStream$BlockDataOutputStream.setBlockDataMode(ObjectOutputStream.java:1756)
    at java.io.ObjectOutputStream.writeObject0(ObjectOutputStream.java:1169)
    at java.io.ObjectOutputStream.writeObject(ObjectOutputStream.java:330)
    at Client.main(Client.java:30)
{% endhighlight %}

The InterruptedIOException comes form SocketOuptputStream.c, which is a private API implemented in C.
Reference code:

[SocketOutputStream in JDK 6 Solaris][link1]

[SocketOutputStream in JDK 7 Soarkis][link2]

<strong>Windows</strong>
The output of the test code in Windows is the same as in Linux. However, since Windows doesn't support interruptible blocking IO, so the JDK implementation is different. It retries sending the data.

[SocketOutputStream in JDK 6 Windows][link3]

[SocketOutputStream in JDK 7 Windows][link4]


<strong>Mac</strong>
The output of the test in Mac is the same as in Linux.

<strong>Java 7</strong>
This issue has gone in Java 7.

<strong>Solution</strong>
Use JVM Option -XX:-UseVMInterruptibleIO to disable the VM Interruptible I/O feature in JDK 6. It is disabled by default in JDK 7.  In JDK 8, it should be depreciated: http://bugs.sun.com/view_bug.do?bug_id=7188233.

-XX:-UseVMInterruptibleIO JVM Option Switch
Solaris users can use the -XX:-UseVMInterruptibleIO JVM Option Switch in order to have applications on Solaris behave similarly under interrupt conditions to applications on Linux or Windows. Refer to 6382902.
(http://www.oracle.com/technetwork/java/javase/documentation/overview-142120.html)


6382902 : VM interrupted I/O feature put on an option switch (sol)
(http://bugs.sun.com/bugdatabase/view_bug.do?bug_id=6382902)

[link1]: http://hg.openjdk.java.net/jdk6/jdk6/jdk/annotate/1a53516ce032/src/solaris/native/java/net/SocketOutputStream.c
[link2]: http://hg.openjdk.java.net/jdk7/jsn/jdk/file/14f50aee4989/src/solaris/native/java/net/SocketOutputStream.c
[link3]: http://hg.openjdk.java.net/jdk6/jdk6/jdk/annotate/1a53516ce032/src/windows/native/java/net/SocketOutputStream.c
[link4]: http://hg.openjdk.java.net/jdk7/jsn/jdk/file/14f50aee4989/src/windows/native/java/net/SocketOutputStream.c