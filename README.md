Intro
-------------------------------------------------------------------------------

Grid2D is a coordinate-based grid for the BLitzmax language, that can be used in games and editors. The appearance and feel is based on the Lightwave 3D modeler grid.

Variable grid sizes and zoom levels are supported, as is snap-to-grid. All grid position related math is vector based. The grid is event driven, no event setup is needed; applications can listen to the update event to re-render the grid.

This is an example of how it looks on a maxgui canvas:

![example](https://dl.dropboxusercontent.com/u/12644619/pics/dev/grid2d_example.png)


Installation
-------------------------------------------------------------------------------

All my modules must be put in a subfolder of `Blitzmax/mod/wdw.mod`.
To install this module, put the `grid2d.mod` folder in `Blitzmax/mod/wdw.mod`. You can do this in the following ways:

1. Clone the repository into your `BlitzMax/mod/wdw.mod` directory using `git clone git://github.com/wiebow/grid2d.mod.git`
2. Click the 'Download Source' button at the top of the Source section of the GitHub repository and extract this into your `BlitzMax/mod/wdw.mod` directory.

After this, simply run `bmk makemods wdw.grid2d` or rebuild the module from your IDE.

Documentation
-------------------------------------------------------------------------------

Documentation is currently viewable on the [project's wiki](http://wiki.github.com/wiebow/grid2d.mod/).

Types
-------------------------------------------------------------------------------

The module consists of a single type: TGrid2D.

Requirements
-------------------------------------------------------------------------------

This module requires the wdw.vector2d module.

Events
-------------------------------------------------------------------------------

The grid emits a EVENT_GADGETPAINT event. The eventsource is the canvas used to render the grid on.

How to use
-------------------------------------------------------------------------------

The grid can be controlled with:

 * right mouse button to drag
 * mouse wheel to zoom in or out
 * `[` to lower grid size, `]` to increase grid size

Code Example
-------------------------------------------------------------------------------

What follows is an example that shows how the grid is used with maxgui.

First, import the module:

    Import wdw.grid2d

Then, create an application type. In this example the New() method also creates the grid. The application also has a GridRender() method in which the grid is rendered and on top of that the application objects can be rendered.

    Local app:TMyApp = New TMyApp
    app.GridRender()

Run the update() method when there is an event.

    While WaitEvent()
        app.Update()
    Wend

This is the application definition. The New() method creates all objects and also registers an event hook. This is not required but it will enable the application to also act on modal events (resize window, for example)

    Type TMyApp
        Field window:TGadget
        Field canvas:TGadget
        Field grid:TGrid2D
		
        Method New()	
            window = CreateWindow("grid",0,0,400,400,Null, ..
			    	WINDOW_CENTER|WINDOW_TITLEBAR|WINDOW_STATUS|WINDOW_RESIZABLE|WINDOW_CLIENTCOORDS)
    		canvas = CreateCanvas(0,0,400,400,w)
	    	SetGadgetLayout(canvas, EDGE_ALIGNED,EDGE_ALIGNED,EDGE_ALIGNED,EDGE_ALIGNED)
		    grid = New TGrid2D
    		grid.SetCanvas(canvas)
		    grid.SetLabel("Example Grid")
		
    		AddHook(EmitEventHook, MyEventHandler, Self)
    	End Method
	
	    Method Update()
    		Select EventID()
			    Case EVENT_APPTERMINATE, EVENT_WINDOWCLOSE
			    End
    		End Select
    	End Method

    	Function MyEventHandler:Object(id:Int, data:Object, context:Object)
		    If data Then TMyApp(context).OnMyEvent(TEvent(data))
	    	Return data
	    End Function

The OnMyEvent() method will call the GridRender() method when a redraw is needed. The EVENT_GADGETPAINT is generated by the grid.

	Method OnMyEvent(event:TEvent)
		If event.source = grid.GetCanvas() And event.id = EVENT_GADGETPAINT
			GridRender()
		EndIf
	End Method
	
	
	Method GridRender()
		grid.Render()
		SetStatusText(w, grid.GetStatusText())	
		grid.UpdateStatusText()
	
		Flip
	End Method
End Type


License
-------------------------------------------------------------------------------

Grid2D is licensed under the MIT license:

    Copyright (c) 2010 Wiebo de Wit.

    Permission is hereby granted, free of charge, to any person obtaining a copy
    of this software and associated documentation files (the "Software"), to deal
    in the Software without restriction, including without limitation the rights
    to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
    copies of the Software, and to permit persons to whom the Software is
    furnished to do so, subject to the following conditions:

    The above copyright notice and this permission notice shall be included in
    all copies or substantial portions of the Software.

    THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
    IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
    FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
    AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
    LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
    OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
    THE SOFTWARE.
