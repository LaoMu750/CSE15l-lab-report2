>
Name:Chaklam Ng
>
Professor Joe
>
CSE 15L
>
4/23/2023
>
___
Welcome to read my cse15l lab report 2.
___
# Part1 Implementing StringServer
>
The following code is the StringServer which is a webserver that will accept path as the following format `/add-message?s=<string>` and it will typically show the string of the path in the web server page.
```
import java.io.IOException;
import java.net.URI;
import java.net.URL;
import java.util.ArrayList;

class Handler implements URLHandler{
    private String message = "";
    @Override
    public String handleRequest(URI url) {
        String[] urlParams = getURLParams(url);
        String[] path = splitPath(url);
        if(path.length==0)
        {
            return "nothing";
        }
        if(path[0].equals("add-message")){
            if(urlParams.length==0)
            {
                return "no params";
            }
            if(urlParams.length>2)
            {
                return "too many params";
            }
            if(urlParams[0].equals("s"))
            {
                message+=urlParams[1]+"\n";
                return message;
            }
            return "unrecognized param:"+urlParams[0];
        }
        return "unrecognized path";
    }
    //helper method
    private String[] getURLParams(URI url){
        if(url.getQuery()!=null)
        {
            return url.getQuery().split("=");
        }
        return new String[0];
    }
    private String[] splitPath(URI url)
    {
        String[] path = url.getPath().split("/");
        ArrayList<String> pathlist = new ArrayList<>();
        for(String p: path)
        {
            if(p.isEmpty()==false)
            {
                pathlist.add(p);
            }
        }
        return pathlist.toArray(new String[0]);
    }
}
public class StringServer {
    public static void main(String[] args) throws IOException
    {
        if(args.length == 0)
        {
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }

}
```
This is similar to the one we have done on lab2 about number server. However, I tried to add some helper methods to help me better build up the Handler for string server.By running the command `javac StringServer.java` and `java StringServer 4688`. Now the system will return me a webpage which will look exactly as the following.
![empty](emptypage.png)
Now the first string path I have tried is `/add-message?s=Hello` and the page would show as the following page which is showing a string word Hello in the page.
![Hello](hellopage.png)
