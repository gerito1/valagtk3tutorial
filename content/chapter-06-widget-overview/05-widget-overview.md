Widget Overview

The general steps to using a widget in GTK are:

[UL]
[ITEM]Use the constructor Gtk.* to create a new widget. by calling the [CODE]Gtk.*()[/CODE] constructor.[/ITEM]

[ITEM]Connect all signals and events we wish to use to the appropriate handlers.[/ITEM]

[ITEM]Set the attributes of the widget.[/ITEM]

[ITEM]Pack the widget into a container using the appropriate call such as 
[CODE]Gtk.Container.add()[/CODE] or [CODE]Gtk.Box.pack_start()[/CODE].[/ITEM]

[ITEM][CODE]Gtk.Widget.show()[/CODE] the widget.[/ITEM]
[/UL]

[CODE]show()[/CODE] lets GTK know that we are done setting the attributes of the 
widget, and it is ready to be displayed. You may also use 
[CODE]Gtk.Widget.hide()[/CODE] to make it disappear again. The order in which 
you show the widgets is not important, but I suggest showing the window last so 
the whole window pops up at once rather than seeing the individual widgets come 
up on the screen as they're formed. The children of a widget (a window is a 
widget too) will not be displayed until the window itself is shown using the 
[CODE]show()[/CODE] method.




Widget Hierarchy

For your reference, here is the class hierarchy tree used to implement widgets. 
(Deprecated widgets and auxiliary classes have been omitted.)

[PRE]
/* TODO: Treeview of widget hierarchy. */
[/PRE]




Widgets Without Windows

The following widgets do not have an associated window. If you want to capture 
events, you'll have to use the EventBox. See the section on the EventBox widget.

[PRE]
  Gtk.Alignment
  Gtk.Arrow
  Gtk.Bin
  Gtk.Box
  Gtk.Button
  Gtk.CheckButton
  Gtk.Fixed
  Gtk.Image
  Gtk.Label
  Gtk.MenuItem
  Gtk.Notebook
  Gtk.Paned
  Gtk.RadioButton
  Gtk.Range
  Gtk.ScrolledWindow
  Gtk.Separator
  Gtk.Table
  Gtk.Toolbar
  Gtk.AspectFrame
  Gtk.Frame
  Gtk.VBox
  Gtk.HBox
  Gtk.VSeparator
  Gtk.HSeparator
[/PRE]

We'll further our exploration of GTK by examining each widget in turn, creating 
a few simple functions to display them.