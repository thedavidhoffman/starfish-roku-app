# Starfish Survival Guide

The intent of this document is to provide a "lay of the land" for how the Starfish Roku app code is constructed, the main entry points, the general conventions used, and common components. Think of it as your tour guide. Since this is a fork of the official Jellyfin Roku App repo, most of this will simply be documenting how that works, and then changes made to it. This document will also cover learnings on how Roku App development works.

## Philosophical Approach
The intent of Starfish is to improve the UX/UI of the Jellyfin Roku app, and not to otherwise affect implementation/functionality. This will allow for updating the fork to be synced with the source repo (jellyfin-roku) with less merging. There may come a time (if this mad experiment actually works out) where the goals of this project expand beyond just improving the UX/UI experience, but for now, that is the scope.

So where a .bs file might need to be modified, whenever possible that code will be abstracted into a file that is only part of Starfish and not part of jellyfin-roku, and hopefully only needed to add a call to that abstracted function from an existing .bs file.

## Roku App Specifics

### Roku app development overview

Roku applications are built using SceneGraph and BrightScript.

#### SceneGraph

SceneGraph is xml that defines the markup for the application UI. This is not based on HTML or CSS and neither of those can be used here. Instead ScreenGraph has its own framework and conventions.

The term SceneGraph refers to a design algorithm and associated programming constructs that are widely used in computer graphics systems, such as video games. A SceneGraph uses a tree structure of image element nodes to define an interactive scene that is traversed to render an image, with the position of each node in the tree determining the z-axis rendering of the node image element; nodes lower in the tree structure are rendered over nodes higher in the tree. Each node in the tree structure is an object whose state is stored as attributes in a set of fields. As the tree is traversed, the rendering state is accumulated, so that the state of a parent node can control how the child nodes are rendered.

From: https://developer.roku.com/en-gb/docs/developer-program/core-concepts/scenegraph-xml/overview.md

In Roku SceneGraph, nodes can either be renderable or not. Renderable nodes are arranged in the tree structure that is traversed during rendering to draw the scene. Non-renderable nodes are ignored during the render traversal, and are used to describe other information used by the scene, such as timers, animations, and data to be displayed. In general, the term node will be used interchangably for both renderable and non-renderable nodes unless it is important to distinguish between the two.

In the Roku SceneGraph implementation, there are only three basic types of nodes that actually draw something.
- A Rectangle node renders a rectangle to the display screen
- A Label node renders a formatted string of text to the display screen
- A Poster node renders a graphical image to the display screen

The power of the Roku SceneGraph architecture is that the three basic renderable node classes, and the supporting non-renderable node classes, can be arbitrarily composed into new node classes that encapsulate more complex appearance and behavior, including common user interface widgets such as buttons, lists, grids, virtual keyboards, and dialogs.

#### BrightScript

BrightScreen is a mix of VB and JavaScript and serves as the programming language for a Roku app.

## Code Overview

### manifest
The manifest file at the root of the project defines:
- The Roku app title
- The Roku app version
- The artwork tile that is displayed when installed and displayed in the app grid on a Roku device
- The Roku app splash screen
- Other configuration options

### Main.bs
Serves as the "Main" entrypoint of a Roku app.
It kickstarts the application and has a while loop that listens to the message port, and takes actions based on messages it receives.

### MainActions.bs
Common subroutines for handling actions such as onPlayButtonClicked, onTrailerButtonClicked.

### MainEventHandler.bs
Top level event handlers.

### ShowScenes.bs
Renders scenes, aka UI pages/elements.
