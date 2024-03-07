# data-communication-and-network-assignment1

Problem 1:
Consider the code we presented in class which consisted of an Echo Client/Server application (Example3.zip posted in the Module 5 folder on Blackboard). In this problem you are required to add a simple improvement on the Client GUI code to make it start with a simple JFrame form asking the user to enter the IP Address of the Server and the Port number the Server is listening on. The form could be something like this:


 


When the client submits the form, the actual client window should appear and connect to the Echo Server.
Hint: Send the variables from the jTextFields of the first form as constructor arguments to the second form.


Problem 2:
Create a Java client code to submit the username/password to a server code. This is done when the client user clicks on the Login button. On the server side, the code will check if the username and password received from the client are valid. Let the valid username/password pairs be stored in an array of Strings in the server code. The array should consist of 10 pairs and having the following structure:
String [] logins=new String[]{“user1”,”pass1”,”user2”,”pass2”, …, “user10”,”pass10”} 
The server should check if the username/password received from the client is part of its array. If this is the case, it sends a “yes” response to the client, else it sends a “no” response. When the client receives the server’s response, it will inform the user via a showMessageDialog message if the authentication is successful or not.

 
In this lab, we’ll explore several aspects of the HTTP protocol: the basic GET/response interaction, retrieving large HTML files, and retrieving HTML files with embedded objects. Support your answers of the questions in this lab with screenshots taken on your own computer.

1. The Basic HTTP GET/response interaction

Let’s begin our exploration of HTTP by downloading a very simple HTML file - one that is very short, and contains no embedded objects.  Do the following:
1.	Start up your web browser.
2.	Start up the Wireshark packet sniffer, as described in the lecture (but don’t yet begin packet capture).  Enter “http” (just the letters, not the quotation marks, and in lower case) in the display-filter-specification window, so that only captured HTTP messages will be displayed later in the packet-listing window.  (We’re only interested in the HTTP protocol here, and don’t want to see the clutter of all captured packets).   
3.	Wait a bit more than one minute (we’ll see why shortly), and then begin Wireshark packet capture.
4.	Enter the following to your browser
http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file1.html
Your browser should display the very simple, one-line HTML file.
5.	Stop Wireshark packet capture.

Your Wireshark window should look similar to the window shown in Figure 1.
 

Figure 1: Wireshark Display after http://gaia.cs.umass.edu/wireshark-labs/ HTTP-wireshark-file1.html has been retrieved by your browser

The example in Figure 1 shows in the packet-listing window that two HTTP messages were captured: the GET message (from your browser to the gaia.cs.umass.edu web server) and the response message from the server to your browser.  The packet-contents window shows details of the selected message (in this case the HTTP OK message, which is highlighted in the packet-listing window).  Recall that since the HTTP message was carried inside a TCP segment, which was carried inside an IP datagram, which was carried within an Ethernet frame, Wireshark displays the Frame, Ethernet, IP, and TCP packet information as well.  We want to minimize the amount of non-HTTP data displayed (we’re interested in HTTP here, and will be investigating these other protocols is later labs), so make sure the boxes at the far left of the Frame, Ethernet, IP and TCP information have a plus sign or a right-pointing triangle (which means there is hidden, undisplayed information), and the HTTP line has a minus sign or a down-pointing triangle (which means that all information about the HTTP message is displayed).

(Note: You should ignore any HTTP GET and response for favicon.ico.  If you see a reference to this file, it is your browser automatically asking the server if it (the server) has a small icon file that should be displayed next to the displayed URL in your browser.  We’ll ignore references to this pesky file in this lab.).

By looking at the information in the HTTP GET and response messages, answer the following questions.
1.	Is your browser running HTTP version 1.0, 1.1, or 2?  What version of HTTP is the server running?
2.	What languages (if any) does your browser indicate that it can accept to the server?
3.	What is the IP address of your computer?  What is the IP address of the gaia.cs.umass.edu server?
4.	What is the status code returned from the server to your browser?
5.	When was the HTML file that you are retrieving last modified at the server?
6.	How many bytes of content are being returned to your browser?
7.	By inspecting the raw data in the packet content window, do you see any headers within the data that are not displayed in the packet-listing window?  If so, name one.

In your answer to question 5 above, you might have been surprised to find that the document you just retrieved was last modified within a minute before you downloaded the document. That’s because (for this particular file), the gaia.cs.umass.edu server is setting the file’s last-modified time to be the current time, and is doing so once per minute. Thus, if you wait a minute between accesses, the file will appear to have been recently modified, and hence your browser will download a “new” copy of the document.


2. Retrieving Long Documents

In our examples thus far, the documents retrieved have been simple and short HTML files. Let’s next see what happens when we download a long HTML file.  Do the following:
•	Start up your web browser, and make sure your browser’s cache is cleared.
•	Start up the Wireshark packet sniffer
•	Enter the following URL into your browser
http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file3.html
Your browser should display the rather lengthy US Bill of Rights.
•	Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed. 

In the packet-listing window, you should see your HTTP GET message, followed by a multiple-packet TCP response to your HTTP GET request.  Make sure you your Wireshark display filter is cleared so that the multi-packet TCP response will be displayed in the packet listing. 

This multiple-packet response deserves a bit of explanation.  Recall from the lecture that the HTTP response message consists of a status line, followed by header lines, followed by a blank line, followed by the entity body.  In the case of our HTTP GET, the entity body in the response is the entire requested HTML file.  In our case here, the HTML file is rather long, and at 4500 bytes is too large to fit in one TCP packet.  The single HTTP response message is thus broken into several pieces by TCP, with each piece being contained within a separate TCP segment (see Figure 1.24 in the text). In recent versions of Wireshark, Wireshark indicates each TCP segment as a separate packet, and the fact that the single HTTP response was fragmented across multiple TCP packets is indicated by the “TCP segment of a reassembled PDU” in the Info column of the Wireshark display.

Answer the following questions:
8.	How many HTTP GET request messages did your browser send?  Which packet number in the trace contains the GET message for the Bill or Rights?
9.	Which packet number in the trace contains the status code and phrase associated with the response to the HTTP GET request?
10.	What is the status code and phrase in the response?
11.	How many data-containing TCP segments were needed to carry the single HTTP response and the text of the Bill of Rights?


3. HTML Documents with Embedded Objects

Now that we’ve seen how Wireshark displays the captured packet traffic for large HTML files, we can look at what happens when your browser downloads a file with embedded objects, i.e., a file that includes other objects (in the example below, image files) that are stored on another server(s).

Do the following:
•	Start up your web browser, and make sure your browser’s cache is cleared, as discussed above.
•	Start up the Wireshark packet sniffer
•	Enter the following URL into your browser
http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html
Your browser should display a short HTML file with two images. These two images are referenced in the base HTML file.  That is, the images themselves are not contained in the HTML; instead the URLs for the images are contained in the downloaded HTML file. As discussed in the textbook, your browser will have to retrieve these logos from the indicated web sites.   Our publisher’s logo is retrieved from the gaia.cs.umass.edu web site.   The image of our 8th edition cover (one of our favorite covers) is stored at a server in France. 
•	Stop Wireshark packet capture, and enter “http” in the display-filter-specification window, so that only captured HTTP messages will be displayed. 

Answer the following questions:
12.	How many HTTP GET request messages did your browser send?  To which Internet addresses were these GET requests sent?
13.	Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel?  Explain.
