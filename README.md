# WEBSRVUTL
Webservice Utilities running on IBM i for providing Web Services and running Webapplications based on AJAX-Requests

## Description

The Library WEBSRVUTL with the Service Program WERSRVUTL gives RPG-Programmers a fast and easy way to provide Web Services powered by IBM i.

Rainer Ross is the developer of the hotel search engine www.myhofi.com - this application is powered by IBM i, built with HTML5, CSS3, JavaScript, Db/2 inMemory, Symmetric Multiprocessing and runs on the server side with pure free RPG Web Services. myhofi.com was awarded in 2015 with the IBM Innovation Award.

## ReadyToClickExamples

Providing JSON www.myhofi.com/myapp/websrv02.pgm?id=1
```
{
    "success": true,
    "errmsg": "",
    "items": [
        {
            "id": 1,
            "name": "MINERALÖL-TANKSTELLE",
            "country": "DE",
            "zip": "12559",
            "city": "BERLIN",
            "street": "GOETHESTR. 8",
            "sales": 535647.59,
            "credit": 5000.00,
            "balance": 1650.00,
            "date": "2015-02-06"
        }
    ]
}
```

Providing XML www.myhofi.com/myapp/websrv01.pgm?id=1
```
<data>
	<item>
		<id>1</id>
		<name>MINERALÖL-TANKSTELLE</name>
		<country>DE</country>
		<zip>12559</zip>
		<city>BERLIN</city>
		<street>GOETHESTR. 8</street>
		<sales>535647.59</sales>
		<credit>5000.00</credit>
		<balance>1650.00</balance>
		<date>2015-02-06</date>
	</item>
</data>
```

Webapplication with AJAX-Request to the JSON-Webservice www.myhofi.com/devhtm/websrv03.html

![capture20170813131922025](https://user-images.githubusercontent.com/10383523/29249116-26fb77dc-802a-11e7-8545-9011d20df3f0.png)

* The AJAX-Request is powered by the JavaScript UI-Library www.webix.com
```
webix.ajax().post("/myapp/websrv01.pgm", {id:0},
    function(text, data) {
    }
);
```

## Software Prerequisites

License Programs

* 5770SS1 Option 3 – Extended Base Directory Support
* 5770SS1 Option 12 – Host Servers
* 5770SS1 Option 30 – Qshell
* 5770SS1 Option 33 – PASE
* 5770SS1 Option 34 – Digital Certificate Manager
* 5770SS1 Option 39 – Components for Unicode
* 5770TC1 - TCP/IP	
* 5770JV1 - Java
* 5770DG1 – HTTP-Server: Apache 2.4.12

Non-License Software (open source)

* YAJL from Scott Klement (create and parse JSON) - Download [here](http://www.scottklement.com/yajl/)

## How to install

* Create a library  `CRTLIB LIB(WEBSRVUTL) TEXT('Webservice Utilities')`
* Create a source physical file `CRTSRCPF FILE(WEBSRVUTL/QCLPSRC)`
* Create a source physical file `CRTSRCPF FILE(WEBSRVUTL/QCPYSRC)`
* Create a source physical file `CRTSRCPF FILE(WEBSRVUTL/QMODSRC)`
* Copy the files from `QCLPSRC, QCPYSRC, QMODSRC` to your SRCPF's
* Compile the CL-Program `CRTBNDCL PGM(WEBSRVUTL/WEBSRVUTLC) SRCFILE(WEBSRVUTL/QCLPSRC)`
* Create the Binding Directory and the Service Program `CALL PGM(WEBSRVUTL/WEBSRVUTLC)` 

## Start and stop the HTTP-Server ADMIN Instance

* Start HTTP-Admin  `STRTCPSVR SERVER(*HTTP) HTTPSVR(*ADMIN)`
* Stop HTTP-Admin `ENDTCPSVR SERVER(*HTTP) HTTPSVR(*ADMIN)`

## Create a new HTTP-Server Instance `MYSERVER`

* Open your browser and start the IBM i HTTP-Admin: http://yourIP:2001/HTTPAdmin
* Create the new HTTP-Server Instance
```
Server name:        MYSERVER
Server description: My new Webserver
Server root:        /www/myserver
Document root:      /www/myserver/htdocs
IP address:         All IP addresses
Port:               8010
Log directory:      /www/myserver/logs
Access log file:    access_log
Error log file:     error_log
Log maintenance     7 days
```
* Start HTTP-Server Instance MYSERVER `STRTCPSVR SERVER(*HTTP) HTTPSVR(MYSERVER)`

* Verify that MYSERVER is running `WRKACTJOB SBS(QHTTPSVR)`
![capture20170813140950764](https://user-images.githubusercontent.com/10383523/29249537-6410cea4-8031-11e7-8c9f-0edefbefac4a.png)

* Call the example Homepage from your browser `http://yourIP:8010/index.html`

## Create your first website

* Open your favorite editor create a new file named `MyFirstWebsite.html` in the `/www/myserver/htdocs` folder and copy https://github.com/RainerRoss/WEBSRVUTL/blob/master/HTML/MyFirstWebsite.html into the `MyFirstWebsite.html` file
#### Make sure that `MyFirstWebsite.html` has the CCSID 1208 (UTF-8)
* Show the files in the folder htdocs `wrklnk '/www/myserver/htdocs/*'`
* Select 8 on `MyFirstWebsite.html` and check the CCSID
* Change the CCSID `CHGATR OBJ('/www/myserver/htdocs/MyFirstWebsite.html') ATR(*CCSID) VALUE(1208)`
* Call `MyFirstWebsite.html` from your browser `http://yourIP:8010/MyFirstWebsite.html`
