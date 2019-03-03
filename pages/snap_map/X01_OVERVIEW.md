---
title: Snap Builder Quick start Intro [WIP]
keywords: snap
sidebar: snap_sidebar
permalink: index.html
folder: snap
---

## Introduction

The snap builder is a new type of layout method for Dungeon Architect.  It builds a dungeon by stitching together pre-built rooms (called modules) that were designed in separate level files.  

In this page, we'll walk through the creation process.  

{% include note.html content="This page is meant to be the placeholder till the rest of the document is complete.  The info in this page should be useful to get you started" %}


---
## Prepare the dungeon actor

Create a new scene and drop in a Dungeon Actor

{% include inline_image.html file="overview/drop_da.png" %}


Select the Snap Map builder
{% include note.html content="Make sure it is SnapMap and not Snap builder" %}

{% include inline_image.html file="overview/select_dungeon_builder.png" %}


We'll need to fill this later on

{% include inline_image.html file="overview/module_flow_assets_missing.png" %}

---

## Create a Module Database

Create a new module database in the Content Browser

{% include inline_image.html file="overview/create_module_database.png" %}

Double click to open it.  This is the editor for the module database. Here is where we'll register our module level files when we create them.  
{% include inline_image.html file="overview/module_database_editor.png" %}

Close this editor window

---

## Creating Module Level File

A module is a pre-designed room. You design a module in separate level files and you specify connection points (usually doors) that should be used for stitching the doors together.  


Right click on the content browser and create a new level file (Right click so the new level file is empty as we don't want the skybox, lights etc)

{% include inline_image.html file="overview/create_module_asset.png" %}

These level files are empty and don't have any lights.  It order to make it easier to design our rooms in these levels,  We'll create a new level (temporary) and edit our module level files from there

Create a new level from *File > New Level* and choose the default one

{% include inline_image.html file="overview/open_levels_window_0.png" %}

Open Levels window 

{% include inline_image.html file="overview/open_levels_window.png" %}

In the Levels window, drag drop our module level and double click on it to make sure it is the currently selected level.  If the level is currently selected, all dropped actors will be placed on it

{% include inline_image.html file="overview/levels_window_drag_drop.png" %}

---

## Designing your Modules

Drag drop and design your module.  Dungeon Architect comes with a rapid prototyping toolset that makes it easy to quickly build your rooms.  We will use this in the sample, however feel free to use your own assets

{% include inline_image.html file="overview/proto_mesh1.png" %}

{% include inline_image.html file="overview/proto_mesh2.png" %}

Make sure the scale and movement is snapped to something like 1 and 100 respectively.  This makes the adjustment easier

{% include inline_image.html file="overview/proto_mesh3.png" %}

Complete the design of your room.   We've left holes where there could be a possible entry / exit.  Here, we will place a connection actor to let Dungeon Architect know that it can stitch other modules from this location.

{% include inline_image.html file="overview/proto_mesh4.png" %}

Make sure all your meshes are inside the module level and not the temporary level we created. Do this by clicking the eye icon on the Levels window and make sure everything dissapears if we hide the module level

{% include inline_image.html file="overview/proto_mesh5.png" %}

---
Go ahead and design another module for our corridor.   

{% include note.html content="Make sure you are editing the correct level file by double clicking on the level name in the Levels window (it should turn bold)" %}

{% include inline_image.html file="overview/proto_mesh9.png" %}

{% include inline_image.html file="overview/proto_mesh6.png" %}

---

## Connections

A Snap Connection tells DA how to stitch the room modules together.  They are usually the Door Entry / Exits.   First create a Connection asset and specify the art asset to use for doors and walls.  

If DA stitches another module through that connection point, it would place the specified Door mesh (or blueprint). Otherwise it would fill up the gap with the specified Wall mesh (or blueprint)

---

Right click on the content browser and create a connection asset

{% include inline_image.html file="overview/connection_1.png" %}

Double click and open up the editor.  Here you specify the mesh or blueprint to use for the Door and Wall.

Your wall mesh thickness should be the same as the wall thickness you used while designing the module.  (we used a thickness of 100 in the above sample)

Specify the proto cube mesh for the walls

{% include inline_image.html file="overview/connection_2.png" %}

{% include inline_image.html file="overview/connection_3.png" %}
 
---
Adjust the size so it is 400x100x400. Always make sure the red arrow points outwards and the wall mesh is behind it (because that is how we are going to align our connection actor later on)
 
{% include inline_image.html file="overview/connection_4.png" %}

We can specify the door blueprint as well but we'll leave it empty in this example. When designing your door meshes, make sure the thickness is twice the thickness of the walls (since we account for the adjacent room as well and the thickness is aligned such that it is in the middle of the red arrow)

Close the connection editor window

---

Drag and drop the connection asset on the door opening

{% include inline_image.html file="overview/connection_5.png" %}


Make sure the alignment arrow is pointing outwards and is on the edge of the screen

{% include note.html content="It is a good practice to design with the snap settings in the editor enabled" %}

{% include inline_image.html file="overview/proto_mesh3.png" %}

{% include inline_image.html file="overview/connection_6.png" %}

Repeat by drag-dropping on all the door openings.   Do this for all the other modules as well (like the corridor module)

{% include inline_image.html file="overview/connection_7.png" %}


---

## Register the Modules

It's time to register our modules in the Module Database.

Open the module database editor

{% include inline_image.html file="overview/module_db_asset.png" %}

{% include inline_image.html file="overview/register_module_db_1.png" %}

---
Click the **Build Module Cache** button after you've modified the module database.  This is an optimization step so your dungeons build fast at runtime. You'll get a warning on screen DA if detects that they are out of date


{% include inline_image.html file="overview/register_module_db_2.png" %}

{% include important.html content="Do not forget the above step. It is important" %}

---

## Dungeon Flow

A Dungeon Flow graph allows you to control the layout of your dungeons using Graph Grammars.   You can generate interesting graphs with simple rules 


Create a new Dungeon Flow Asset

{% include inline_image.html file="overview/flow_1.png" %}

---

Add two new nodes **Room** and **Corridor**.  You can change the name of the nodes from the details panel.

These names map to the names you specified on the Module database (Room and Corridor)

{% include inline_image.html file="overview/flow_2.png" %}

{% include inline_image.html file="overview/flow_3.png" %}


---

Select the *Start Rule* and on the RHS,  drop in a few Room nodes like this:

{% include inline_image.html file="overview/flow_7.png" %}
{% include inline_image.html file="overview/flow_4.png" %}

{% include note.html content="Cycles are not supported by the SnapMap builder" %}


---

Execute the rule and see how the final graph is generated. You do this by clicking the Run icon on the Execution graph panel

{% include inline_image.html file="overview/flow_5.png" %}

{% include inline_image.html file="overview/flow_6.png" %}

---

We'd like to insert Corridors between the rooms.   Create another rule and give it a name (e.g. *Insert Corridors*)

{% include inline_image.html file="overview/flow_8.png" %}

On the LHS, we want to find a patterns where two rooms are connected to each other like this (Room -> Room) and have it replaced with (Room -> Corridor -> Room)

The Graph Grammar will find a pattern you specify on the LHS and replace it with the one you specify on the RHS

The Indices on the nodes (e.g. Room:0, Room:1) are important that helps in correct mapping.   Since we properly specified 0 and 1 indices on the RHS, it knows the direction of the newly created links to the corridor.   This will be covered in detail in the full documentation soon

---

You control how your rules are run from the **Execution Graph**.  Drag drop your newly created **Insert Corridor** rule on to the execution graph and connect it after the *Start Rule*.  

{% include inline_image.html file="overview/flow_9.png" %}

Select the newly placed node and from the details panel, change the execution mode to Iterate and set the count to 2 or 3 (This makes the rule run multiple times since the newly replaced Room nodes wont map with the adjacent older Room nodes by design and need to be run again)

{% include inline_image.html file="overview/flow_A.png" %}

---

Execute the grammar and you'll now see corridors between your rooms

{% include inline_image.html file="overview/flow_B.png" %}

We will use this Dungeon Flow graph grammar to generate our snap dungeons

---

## Generating the Dungeon

Assign the **Module Database** and **Dungeon Flow Graph** assets to the Dungeon Actor

{% include inline_image.html file="overview/finalize_1.png" %}

Hit **Build Dungeon**.  Click Randomize and try other configurations.  Change the Dungeon Flow graph and experiment further

{% include inline_image.html file="overview/finalize_2.png" %}

---

Enable Debug Draw for more visual info

{% include inline_image.html file="overview/finalize_3.png" %}

{% include inline_image.html file="overview/finalize_4.png" %}

---

## Level Streaming

The snap builder fully supports Level Streaming. It would Stream In and Out module level files depending on the layout graph and customizable visiblity depth

Select the Dungeon Actor and enable level streaming

{% include inline_image.html file="overview/finalize_5A.png" %}

---

Build a new Dungeon at runtime on BeginPlay of the Level's blueprint

{% include inline_image.html file="overview/finalize_5B.png" %}

{% include note.html content="You can get the reference to the Dungeon1 node in the above blueprint by first selecting the dungeon in the level editor, then right click on the level blueprint" %}

---

Destroy your existing dungeon. Hit play and the nearby modules will be streamed in / out as you move through dungeons.  This helps with maintaining a smooth framerate with fully dynamic lighting

{% include inline_image.html file="overview/finalize_6.png" %}



## Sample Game

Explore the sample game which contains many more advanced features (minimaps, key / lock demo, NPCs, navigation etc)

[Game Source](https://drive.google.com/file/d/1ogDj-hxcYECm8CxSH_knxoHC4jP-qll6/view?usp=sharing)

[Game Binaries](https://drive.google.com/file/d/1r7o1Mz-Wf_0H4mp_Rt8AqxkBG6r29loM/view?usp=sharing)

**Sample Game**

[![Sample Game](https://img.youtube.com/vi/woKkR6P_sHY/0.jpg)](https://www.youtube.com/watch?v=woKkR6P_sHY)


**MiniMap Demo**

[![MiniMap Demo](https://img.youtube.com/vi/R5Lcx9YSYpo/0.jpg)](https://www.youtube.com/watch?v=R5Lcx9YSYpo)


**Multi-Level Dungeons**

[![Multi-Level Dungeons](https://img.youtube.com/vi/9Crl9tGkHXA/0.jpg)](https://www.youtube.com/watch?v=9Crl9tGkHXA)





