_This guide is heavely inspired by a guide by @depwl9992. The guides are very similar, but i wanned to include some things i wish i knew, or had problems with when i was reading his guide https://gist.github.com/depwl9992/626bf2c20269abe9cda79eeab0de58af._

***I will not show how to download and install dynmap, so make sure to download and get it working before doing the steps bellow. This guide is for rendering a additional map of the nether roof, while also showing some other useful commands.***

# Adding a custom "Flat" perspective
To render the nether roof, you first need to make a new perspective. This first map will be flat (2d).
1. Save and stop the server.    
2. Open the `custom-perspectives.txt` file located in `server\plugins\dynmap\custom-perspectives.txt`.
3. On line 3 in this file it should say "**perspectives:**", paste this text on the line bellow:
```yaml
- class: org.dynmap.hdmap.IsoHDPerspective
  name: nether_top_map_hires
  maximumheight: 256
  minimumheight: 128
  inclination: 90
  scale: 16
  azimuth: 180
```
Now save and close the file. The name can be anything you want, but you have to use that same name later.
This Perspective will be in "**hires**". If you want to change to a smaller resolution, you just have to change "**Scale**" to a smaller value.
```
scale: 16 = hires
scale: 8 = medres
scale: 4 = lowres
scale: 2 = vlowres
```
In this map the bedrock layer will be visible. If you don't want the bedrock to be visible in the map, just change the "**minimumheight**" value from **128** to **129**.

4. Start the server. \
In the next few steps you will execute a few commands. You can do these commands either in the **server console**, or **in-game**. I prefer to do it in-game because **tab** lets you finish the command without typing every single symbol.  

5. Pause live map updates: 
```console
dynmap pause all
``` 

6. Add the map:
```console
dmap mapadd world_nether:nether_roof title:"Nether Roof" perspective:nether_top_map_hires
```
This adds the actual **map**. If your **world** file is called anything other than "**world**" make sure to change "**world_nether**" in the command to your actual world name. My world file is called "JonasServer" So the command i have to write is "dmap mapadd JonasServer_nether:nether_roof title:"The Roof" perspective:nether_top_map_lowre". You can also change the **title** and **name** to anything you want. 

7. Unpause live map update:
```console
dynmap pause none
```

8. Stop rendering nether:
```console
dynmap cancelrender world_nether
```
Again, if your world name is different remember to change it in the command.

9. Finally, start rendering the new map you just made:
```console
dynmap fullrender world_nether:nether_roof
```
And that goes for this one too, remember to use the right world name. Also, i wouldn't reccomend typing this one in-game, as you will be spammed in chat every few seconds when it renders the map. Use the server console instead.  \
\
\
***You should now have a working flat nether roof map! The icon will probably be blank, or it will use one of the other nether icons. The background of the map will also be black. If you wish to chagne the icon, or background follow the next steps.*** 

If you have any errors when starting the servers after making these changes, copy all the text in the `custom-perspectives.txt` file into a YAML verifier and check for errors. This is the one i use: https://www.yamllint.com/.
A single invisible "space" too much can cause it to not work. Copy-pasting the code will sometimes break it for some reason, so make sure there is no errors in the file.



# Changing map icon
To see all the available icons open the **images** folder located here: `server\plugins\dynmap\web\images`. You can use any of these icons, or add your own. Just remember to make the resolution of the image **16x16** pixles. Find an image you want to use, then follow the steps bellow:

![Dynmap images](https://i.imgur.com/g4yAkv2.png)

1. Pause live map updates: 
```console
dynmap pause all
```
2. Set the icon:
```console
dmap mapset world_nether:nether_roof icon:images/(The image you want)
```
simply replace "**(The image you want)**" with the name of the image you want to use. On my map i uploaded an image of a flat bedrock block for the flat map, and a 3d bedrock block for the 3d map. So when i ran the command it looked like this: "dmap mapset JonasServer_nether:nether_roof icon:images/bedrock.png".


# Changing map background
You can change the map background to anything you want, but im gonna keep it simple and just add the same red color that is used in the default nether maps. 

1. Stop the server.
2. Open the **worlds** folder located here: `server\plugins\dynmap\worlds.txt`
3. Located the map you just made inside the `worlds.txt` file. You might have to scroll down a bit to find the nether maps. Right under the default nether maps you should see the nether roof map.
4. Once you find the map simply add this line of code in between **tilescale**, and **append_to_world**:
```yaml
background: '#300806'
```
5. Start the server again, and the nether roof map should now have a dark red background.

# Rendering the nether roof with a 3D/Surface camera angle
Here is how you render the nether roof with a 3d view. You can either make a new perspective and map again, or you can change the flat map you made earlier. I would recomend making a new map.

If you are making a new map, follow the steps for "**Adding a custom "Flat" perspective**" again, but come back here after doing step 3. If you are not making a new map you can simply edit the text you pasted in `custom-perspectives.txt` earlier.

1. Inside the `custom-perspectives.txt` file, change **inclination** to **30**. This will make the camera angle the same as the default **Overworld_Surface** map. You can also change **azimuth** to change the rotation. **135** is used for **Overworld_Surface** by default. If you want any other direction, simply look at the chart bellow, and copy that.
```
azimuth: 0        = North
inclination: 30   
-------------------------------
azimuth: 45       = North-East
inclination: 30   
-------------------------------
azimuth: 90       = East
inclination: 30   
-------------------------------
azimuth: 135      = South-East
inclination: 30   
-------------------------------
azimuth: 180      = South
inclination: 30   
-------------------------------
azimuth: 225      = South-West
inclination: 30   
-------------------------------
azimuth: 270      = West
inclination: 30   
-------------------------------
azimuth: 315      = North-West
inclination: 30   
```

# Displaying markers on sidebar
There is actually an option to display markers on the sidebar, but this function is hidden. Here is a quick turtorial on how to display them

![Dynmap markers on sidebar](https://i.imgur.com/JxIbZaC.png)

1. Open `Server\plugins\dynmap\configuration.txt`
2. Paste the following lines under the other component definitions
```yaml
- class: org.dynmap.ClientComponent
  type: sidebarmarkers
  title: 'Markers (Current world)'
```
This will display all the markers in the same dimention as the map currently selected. You can hover your cursor on the icons to see the marker name, or click the markers to jump to the marker on the map. Personally i don't like the title to be displayed on the sidebar, but i couldn't figure out how to remove it. Even if i removed the title line from the file it still displayed the title. Therefor changed the title to display the seed instead on my own map.

# Displaying the marker titles
I also like to see the marker title on my map. This can easily be enabled in the `configuration.txt` file.
![Display marker titles](https://i.imgur.com/rfn8uhD.png)
1. Open `configuration.txt`
2. Scroll down untill you find the marker component
```yaml
- class: org.dynmap.MarkersComponent
    type: markers
    showlabel: false
    enablesigns: false
    default-sign-set: markers
    showspawn: true
    spawnicon: world
    spawnlabel: Spawn
    showofflineplayers: false
    offlinelabel: Offline
    offlineicon: offlineuser
    offlinehidebydefault: true
    offlineminzoom: 0
    maxofflinetime: 30
    showspawnbeds: false
    spawnbedlabel: Spawn Beds
    spawnbedicon: bed
    spawnbedhidebydefault: true
    spawnbedminzoom: 0
    spawnbedformat: "%name%'s bed"
    showworldborder: true
    worldborderlabel: Border
```
3. Change
```yaml
showlable: false
```
to
```yaml
showlable: true
```

# Deleting maps
1. Find the **ID** of the map you want to delete:
```console
dmap worldlist
```
2. Pause live map updates: 
```console
dynmap pause all
``` 
3. Delete the map, use the map **ID/Name** you found in step 1. In this example i will show how to delete the map made earlier in this guide:
```console
dmap mapdelete world_nether:nether_roof
```
4. Unpause live map update:
```console
dynmap pause none
```
