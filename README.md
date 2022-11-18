
# How-to-Install-TERA-Server

The following guide assumes that you have a dedicated computer to install the server files (this should work on a vm too, but more steps are needed)
Keep in mind, the purpose of this guide is pointing you to the right direction consolidating the most useful info about the tera server configuration just in one place.

# Contents

1. Server Files
2. Requirements
3. Installing SQL Server
4. Restore Databases SQL Server
5. Installing MySQL Server
6. Create Account Database MySQL Server
7. Server Setup
7.1 arb_gw
7.2 hub and hub_gw
7.3 DeploymentConfig.xml
8. API
9. Starting the Server
10. How to get and change the IP address from launcher
11. Enable item Claim (Box/Steer Server) (Optional)
12. Enable TERA Shop Features (Optional)
13. Enable Launcher Patching (Optional)
14. Enable Proxy (Optional)
15. Setting Up WebApp (Optional)
16. Useful Releases
17. VMS
18. Useful Guides for Beginners
19. Extra
20. FAQ

## 1. Server Files
 * [92.03](https://disk.yandex.ru/d/Fob3M8BffjcTrg)
 * [92.04](https://disk.yandex.ru/d/GcB_CyNARHo3qw)
 * [100.02](https://drive.google.com/drive/folders/1kNkxB8H9_ROIwaMIav2lCtfPP9Wo9NB2), [Link 2](https://disk.yandex.ru/d/mIKWjplKyYiliA?w=1)
## 1.1 Client Files
* [92.03](https://forum.ragezone.com/f797/release-tera-v92-03-retail-1193481-post9079748/#post9079748)
* [92.04](https://forum.ragezone.com/f797/tera-server-92-04-retail-1206066-post9129907/#post9129907)
* [100.02](https://mega.nz/folder/wx8AEAIC#JdSyKwOlpC6PIV8sffxzGA)

## 2. Requirements
A PC with at least 96GB of ram... but i know almost all of us doesn't meet this requerement, just set a page file in a ssd with a minimum size of 80GB. Also you must have a system locate on English or Korean

* MSSQL dev [2017](https://go.microsoft.com/fwlink/?linkid=853016) or [2019](https://go.microsoft.com/fwlink/?linkid=866662) (2008R2+ and so on works too)
* [SSMS](https://learn.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver16)
* [HeidiSQL](https://www.heidisql.com/download.php)
* [sqlncli](https://www.microsoft.com/en-US/download/details.aspx?id=36434)
* [NodeJS 19.x](https://nodejs.org/dist/v19.1.0/node-v19.1.0-x64.msi)
* [MySQL Server 5.7.38](https://downloads.mysql.com/archives/get/p/25/file/mysql-installer-community-5.7.38.0.msi)
* [Notepad++](https://notepad-plus-plus.org/downloads/) or [VSCode](https://code.visualstudio.com/download)
* [Resource hacker](http://www.angusj.com/resourcehacker/#download)


## 3. Installing SQL Server

Follow [this guide](https://www.guru99.com/download-install-sql-server.html) for install SQL Server; **while you install, do not forget to enable mixed authentication**  
![image](https://user-images.githubusercontent.com/69399372/202078382-ff123fe6-1960-418d-8c55-cf10d9e32880.png)  
**Remember which password you set, this will be the SA user password**

## 4. Restore Databases SQL Server

Open and login into SSMS
Right clic on Databases then clic on restore database  
![image](https://user-images.githubusercontent.com/69399372/202078764-49415e3c-92c6-42a9-a988-7b9b30bd99ef.png)  
Now follow the number order in the next image  
![image](https://user-images.githubusercontent.com/69399372/202078772-2fafc592-e2ed-4c44-acff-cf77404337d0.png)  
![image](https://user-images.githubusercontent.com/69399372/202078788-068579a3-1254-4b28-af72-08975a4cf410.png)  

Now do the same thing for SharedDB, CollectionDB (100.02 only), LogDB_2800 and WebAppDB.

If you want to start with clean databases follow [this guide](https://forum.ragezone.com/f798/how-to-create-clean-databases-1194564/).

## 5. Installing MySQL Server

Nothing especial, just remember which **root password** you set.

## 6. Create Account Database MySQL Server

Create a database named “accountdb_2800” with collation utf8 and execute the following sql query [located here](https://github.com/justkeepquiet/tera-api/blob/master/share/db/00_db_schema.sql) on it  

![image](https://user-images.githubusercontent.com/69399372/202078958-9f464f32-8009-4c28-8dd0-561493d1f04f.png)  
![image](https://user-images.githubusercontent.com/69399372/202079013-231a9398-bdc6-4466-a903-f7b404924d73.png)  
![image](https://user-images.githubusercontent.com/69399372/202079019-3f65354a-b49c-4440-8ad3-c769d58de726.png)  


## 7. Server Setup

#### 7.1 arb_gw

Open and edit config_arb_gw.txt and set like the following image,  

![image](https://user-images.githubusercontent.com/69399372/202079249-d11297b5-f71f-495b-9b1e-3edb7c1c38c7.png)  

```
hub_ip=127.0.0.1
hub_port=11001
game_port=10001
rest_timeout=0
rest_url=http://127.0.0.1:8080/api
rest_url_auth=http://127.0.0.1:8080/
web_shop_url=http://127.0.0.1/tera/ShopAuth?authKey=%s
tba_web_shop_url=http://127.0.0.1/tera/ShopAuth?authKey=%s
thread_cnt=16
```

#### 7.2 hub and hub_gw

Leave hub and hub_gw as is, no changes are needed.

#### 7.3 DeploymentConfig.xml

Located at ~\Executable\Bin\, Open and edit the following lines with your SQL Server **SA** / MySQL **root** credencials  
![image](https://user-images.githubusercontent.com/69399372/202079473-39cb2a6e-bc0a-45a6-989e-1d78aad6cada.png)  

![image](https://user-images.githubusercontent.com/69399372/202079477-43225ecb-3798-405b-a56a-d72b7c2ff119.png)  

**In 92.03/04 CollectionDB doesn't exist.**

Set external ip value to 127.0.0.1 like this
```
<ExternalIp getFromNetworkDevice="true" value="127.0.0.1" />
```

## 8. API

[Main Thread](https://forum.ragezone.com/f797/tera-api-node-js-and-1205579/)
Download and installation guide from [here](https://github.com/justkeepquiet/tera-api).

## 9. Starting the Server

Recommended server startup order (Taken [From](https://forum.ragezone.com/f797/tera-api-node-js-and-1205579/))
1. hub and hub_gw
2. Steer Server services
3. Box Server services
4. TERA API
5. REST (Nexusserver, MatchServer, XigncodeProxy)
6. TopographyServer
7. arb_gw and ArbiterServer
8. WorldServer (PartyMatching, BattleField, DungeonServer)
9. TERA Server Proxy (if used)

You must wail until **''$Server Ready$''** and **''$Registered....''** (100.02) status for enter the game with all features.  

![image](https://user-images.githubusercontent.com/69399372/202079741-f0707e54-32a1-4287-a7e4-d0ffc45ac232.png)  
![image](https://user-images.githubusercontent.com/69399372/202079750-ccf8028a-a907-4b21-843a-63bb33d5d78c.png)  

## 10. How to get and change the IP address from launcher

Download the launcher from [here](https://forum.ragezone.com/f797/tera-api-node-js-and-1205579/)

Open the launcher with resource hacker and go to string table and edit **YOURSERVERIP** in strings 19 and 20  

![image](https://user-images.githubusercontent.com/69399372/202079885-3eeaf1ac-f940-4e0c-a70d-a19098e95e0e.png)  
![image](https://user-images.githubusercontent.com/69399372/202079890-95d3ccb0-4ef1-437e-8939-ad6c75546106.png)  

## 11. Enable item Claim (Box/Steer Server) (Optional)

Follow [this](https://forum.ragezone.com/f797/tera-92-100-steer-server-1206086/) guide

## 12. Enable TERA Shop Features (Optional)

Follow [this](https://forum.ragezone.com/f797/tera-api-node-js-and-1205579-post9137118/#post9137118) guide

## 13. Enable Launcher Patching (Optional)

Follow [this](https://forum.ragezone.com/f797/tera-api-node-js-and-1205579/) guide

## 14. Enable Proxy (Optional)

Follow [this](https://forum.ragezone.com/f797/tera-server-proxy-allowing-gm-1207688/) guide

## 15. Setting Up WebApp (Optional)

Follow [this](https://forum.ragezone.com/f797/tera-level-100-version-1205489-post9125989/#post9125989) guide

## 16. Useful Releases

* [Tera Controller Fix](https://forum.ragezone.com/f797/tera-controller-fix-1205728/)
* [Server Official Documentation](https://forum.ragezone.com/f797/tera-server-official-documentation-1206071/)
* [Server Opcodes 100.02](https://forum.ragezone.com/f797/server-opcodes-p100-02-a-1207231/)
* [Translation Files for TERA 92.03 (ENG,RUS,GER,FRA)](https://forum.ragezone.com/f797/translation-files-for-tera-92-a-1198465/)
* [Tera 92 auto learn skills (excluding common skills)](https://forum.ragezone.com/f797/share-tera-92-auto-learn-1206323/)
* [SQL Script MSSQL 2014](https://forum.ragezone.com/f797/tera-sql-script-for-mssql-1208094/)
* [Datacenter Collection](https://forum.ragezone.com/f797/tera-datacenter-collection-1208296/)

## 17. VMS

* [92.03](https://forum.ragezone.com/f797/92-03-vmware-1198583/)
* [100.02 Option 1](https://forum.ragezone.com/f797/tera-level-100-version-1205489/index17.html#post9127017)
* [100.02 Option 2](https://forum.ragezone.com/f797/tera-100-02-server-vm-1207538/)

## 18. Useful Guides for Beginners

* [How to Enable Halloween, Xmax and other Events](https://forum.ragezone.com/f798/tera-92-03-how-to-1194887/)
* [TERA GM Commands](https://forum.ragezone.com/f798/tera-92-03-gm-commands-1194804/)
* [How to make your server fully PvE](https://forum.ragezone.com/f798/tera-92-03-how-to-1201587/)
* [How to Enable Play Time Event](https://forum.ragezone.com/f798/tera-92-03-100-02-a-1205727/)
* [How to Enable Dressing Room / Fashion Shop](https://forum.ragezone.com/f798/tera-92-03-100-02-a-1205748/)
* [Faster gathering/mining/harvesting speeds](https://forum.ragezone.com/f798/tera-100-02-faster-gathering-1207133/)
* [Faster attack and skill speeds](https://forum.ragezone.com/f798/tera-100-02-faster-attack-1207132/)
* [Enable/Edit Tikat shop](https://forum.ragezone.com/f798/92-03-enable-edit-tikat-1200695/)
* [How to Add Items for the In-Game Calendar (Deprecated)](https://forum.ragezone.com/f798/how-to-add-items-for-1194457/)
* [How to Setup Arbiter Hook 92.03 (Deprecated)](https://forum.ragezone.com/f798/tera-92-03-how-to-1204497/)
* [How to get rid/change of the "noname" Box](https://forum.ragezone.com/f798/how-to-get-rid-change-1201553/)
* [TERA Dungeon ID NA/EU with KR Names](https://forum.ragezone.com/f798/tera-dungeon-id-na-eu-1201551/)
* [How to change experience rate in your server](https://forum.ragezone.com/f798/mini-guide-how-to-change-1200649/)
* [How to configure VM for local network play](https://forum.ragezone.com/f798/tera-100-02-vm-on-1207134/)
* [PDF Primer for Setting Up the Virtual Machine version of Retail 100.02](https://forum.ragezone.com/f798/pdf-primer-for-setting-up-1205925/)
* [How to remove CSN completion from Kaia Anvil Quest](https://forum.ragezone.com/f798/how-to-remove-csn-completion-1209027/)
* [Restoring Torch of the Gods (Kelsaik Nest Hard)](https://forum.ragezone.com/f798/restoring-torch-of-the-gods-1208952/)
* [How to Fix Fall Damage in Manaya's Core Hard](https://forum.ragezone.com/f798/how-to-fix-fall-damage-1209065/)

## 19. Extra

[Official Fan/Press/Art Kits](https://forum.ragezone.com/f797/tera-oficial-fan-press-art-1206065/)

## 20. FAQ

  * **CreateFileMapping ?? (5)**  
  * **Server Version: 366358(branch:Live92_3_TW)Exception: %saccess violation(c0000005) has occurred at XXXXXXXXXXXXXXXX**  

  ![image](https://user-images.githubusercontent.com/69399372/202080307-c64c4bd1-2632-4316-8ee4-b9324da70616.png)  


   **Solution:** Make sure to execute Topography and Worldserver as administrator. Also you must have a system locate on English or Korean  

  * **Grails hags at 83%**  

  ![image](https://user-images.githubusercontent.com/69399372/202080320-95dd1ba7-d51b-41e3-b111-1b242475cc51.png)  


   **Solution:** Stop using the old java API. Look at the step **8. API**  

  * **When you start TERA Client 92.03, xingcode appears and inmediately or mins later it closes the client.**  

   **Solution:** Download and apply this [xingcode patch](https://forum.ragezone.com/f797/release-tera-v92-03-retail-1193481-post9060089/#post9060089)  

  * **You can't enter right now**  

  ![image](https://user-images.githubusercontent.com/69399372/202080345-f6627a5e-57ca-4f2c-9353-12599ab56d75.png)  


   **Solution:** Wait until **''$Server Ready$''** Status in world server.  

  * **Everyone can use gm commands, what can i do?**  

   **Solution:** Open DeploymentConfig.xml and edit **qaServer="true"** to **"false"**

  ```
  <ArbiterServerConfig qaServer="false" validatePacket="true" pveServer="true">  
  ```

   Then check **14. Enable Proxy (Optional)** step  

  * **I'm getting speedhack messages and the server kicks me out. (Contents Cheat Account [xxxx])**

   **Solution:** Open **ServerConfig.xml** and set <Speedhack turnOn=**"true"** to **false**  

  ![image](https://user-images.githubusercontent.com/69399372/202080469-c87e65cd-07ec-4be3-b0fd-06a589cc0b06.png)  

  * **My friends can't connect to my server, which ports i need to open?**  

   **Solution:** Make sure you have your Public / VPN IP address into "loginIp" column of "server_info" table in the Node JS API, then allow the following ports in      your windows fw and router,

    80  
    443 (Assuming you're using a custom webpage in the same machine with ssl enabled)  
    7801 (Assuming you're not using the proxy, if so, allow the port you choose as proxy)  

  * **How to change the server name which appears at lobby?**  

   **Solution:** Open your mysql db, goto "server_info" table and set your server name into "nameString" column  

  * **How i can reduce the RAM comsumption of my server?**  

   **Solution:** Edit ~\Datasheet\ContinentData.xml and make sure every single **initChannelCount="1"** has **"1"** as value  

  * **Arbiter Session Destroyed when world server is loading**  

   **Solution:** This error happens because there's no enough ram available. So, the posible solutions are,  

    Buy more RAM  
    Increase your virtual page  
    Keep trying until it loads correctly  

  * **Laurels don't work, what can i do?**  

   **Solution:** Set an Achievement Season throught WebApp or directly into the "AchievementSeason" table at PlanetDB_2800 database  

  * **Client [] Module [PDL.xml] Version Mismatched! [S:XXXXXX] [C:XXXXXX]**  

  ![image](https://user-images.githubusercontent.com/69399372/202080494-72a05c2f-bc4f-4add-ac19-646e8b03a0f0.png)  

   **Solution:** Client versions are not compatible between updates, so if you downloaded a 100.02 server then you must use a 100.02 client. Using a 115.02 client or superior will not work.  

  * **How i change the gameforge welcome message in client chat box?**  

   **Solution:** Can be edited from StrSheet_SystemMessage.xml (client side)  
   
  
  
    I'm still polishing this and will be more guides coming near the future.  
                                                           
    If your tutorial or release is not present here, it doesn't mean that isn't useful...i just didn't added it yet.  
  
    Credits and Thanks to all RaGEZONE community folks who shared their knowledge and doesn't keep for themselves or for profit.  
                                                           
                                                          

