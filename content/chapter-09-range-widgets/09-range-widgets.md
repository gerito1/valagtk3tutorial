Range Widgets

The category of range widgets includes the ubiquitous scrollbar widget and the 
less common "scale" widget. Though these two types of widgets are generally 
used for different purposes, they are quite similar in function and 
implementation. All range widgets share a set of common graphic elements, each 
of which has its own X window and receives events. They all contain a "trough" 
and a "slider" (what is sometimes called a "thumbwheel" in other GUI 
environments). Dragging the slider with the pointer moves it back and forth 
within the trough, while clicking in the trough advances the slider towards the 
location of the click, either completely, or by a designated amount, depending 
on which mouse button is used.

As mentioned in Chapter 7, Adjustments above, all range widgets are associated 
with an Adjustment object, from which they calculate the length of the slider 
and its position within the trough. When the user manipulates the slider, the 
range widget will change the value of the adjustment.




Scrollbar Widgets

These are your standard, run-of-the-mill scrollbars. These should be used only 
for scrolling some other widget, such as a list, a text box, or a viewport (and 
it's generally easier to use the scrolled window widget in most cases). For 
other purposes, you should use scale widgets, as they are friendlier and more 
featureful.

There are separate types for horizontal and vertical scrollbars. There really 
isn't much to say about these. You create them with the following methods:

[CODE]
  hscrollbar = new Gtk.HScrollbar(Gtk.Adjustment? adjustment);

  vscrollbar = new Gtk.VScrollbar(Gtk.Adjustment? adjustment);
[/CODE]

and that's about it. The adjustment argument can either be a reference to an 
existing Adjustment (see Chapter 7, Adjustments), or nothing, in which case one 
will be created for you. Specifying nothing might be useful in the case, where 
you wish to pass the newly-created adjustment to the constructor function of 
some other widget which will configure it for you, such as a text widget.




Scale Widgets

Scale widgets are used to allow the user to visually select and manipulate a 
value within a specific range. You might want to use a scale widget, for 
example, to adjust the magnification level on a zoomed preview of a picture, or 
to control the brightness of a color, or to specify the number of minutes of 
inactivity before a screensaver takes over the screen.



Creating a Scale Widget
As with scrollbars, there are separate widget types for horizontal and vertical 
scale widgets. (Most programmers seem to favour horizontal scale widgets.) 
Since they work essentially the same way, there's no need to treat them 
separately here. The following functions create vertical and horizontal scale 
widgets, respectively:

[CODE]
vscale = new Gtk.VScale(Gtk.Adjustment? adjustment);

vscale = new Gtk.VScale.with_range(double min, double max, double step);

hscale = new Gtk.HScale(Gtk.Adjustment? adjustment);

hscale = new Gtk.HScale.with_range(double min, double max, double step);
[/CODE]

The adjustment argument can either be an adjustment which has already been 
created with [CODE]new Gtk.Adjustment()[/CODE], or null, in which case, an 
anonymous [CODE]Adjustment[/CODE] is created with all of its values set to 0.0 
(which isn't very useful in this case). In order to avoid confusing yourself, 
you probably want to create your adjustment with a page_size of 0.0 so that its 
upper value actually corresponds to the highest value the user can select. 
The [CODE].with_range()[/CODE] variants take care of creating a suitable 
adjustment. (If you're already thoroughly confused, read the section on Adjustments 
again for an explanation of what exactly adjustments do and how to create and 
manipulate them.)



Functions and Signals (well, functions, at least)

Scale widgets can display their current value as a number beside the trough. 
The default behaviour is to show the value, but you can change this with this 
function:

[CODE]
void Gtk.Scale.set_draw_value(bool draw_value);
[/CODE]

As you might have guessed, draw_value is either [CODE]true[/CODE] or 
[CODE]false[/CODE], with predictable consequences for either one.

The value displayed by a scale widget is rounded to one decimal point by 
default, as is the value field in its Adjustment. You can change this with:

[CODE]
void Gtk.Scale.set_digits(int digits);
[/CODE]

where digits is the number of decimal places you want. You can set digits to 
anything you like, but no more than 13 decimal places will actually be drawn on 
screen.

Finally, the value can be drawn in different positions relative to the trough:

[CODE]
void Gtk.set_value_pos(Gtk.PositionType  pos);
[/CODE]

The argument pos is of type Gtk.PositionType, which can take one of the following 
values:

[PRE]
  Gtk.PositionType.LEFT
  Gtk.PositionType.RIGHT
  Gtk.PositionType.TOP
  Gtk.PositionType.BOTTOM
[/PRE]

If you position the value on the "side" of the trough (e.g., on the top or 
bottom of a horizontal scale widget), then it will follow the slider up and 
down the trough.




Common Range Functions

The Range widget class is fairly complicated internally, but, like all the 
"base class" widgets, most of its complexity is only interesting if you want to 
hack on it. Also, almost all of the functions and signals it defines are only 
really used in writing derived widgets.

/** --DON'T THINK THIS IS IN GTK+3 


Setting the Update Policy

The "update policy" of a range widget defines at what points during user 
interaction it will change the value field of its Adjustment and emit the 
"value_changed" signal on this Adjustment. The update policies are:

GTK_UPDATE_CONTINUOUS
This is the default. The "value_changed" signal is emitted continuously, i.e., 
whenever the slider is moved by even the tiniest amount.

GTK_UPDATE_DISCONTINUOUS
The "value_changed" signal is only emitted once the slider has stopped moving 
and the user has released the mouse button.

GTK_UPDATE_DELAYED
The "value_changed" signal is emitted when the user releases the mouse button, 
or if the slider stops moving for a short period of time.

The update policy of a range widget can be set by casting it using the 
GTK_RANGE(widget) macro and passing it to this function:

[CODE]
void gtk_range_set_update_policy( GtkRange      *range,
                                  GtkUpdateType  policy);
[/CODE] **/



Getting and Setting Adjustments

Getting and setting the adjustment for a range widget "on the fly" is done, 
predictably, with:

[CODE]

Gtk.Adjustment range.get_adjustment();

void range.set_adjustment(Gtk.Adjustment adjustment);
[/CODE]

[CODE]range.get_adjustment()[/CODE] returns a reference to the adjustment to 
which [CODE]range[/CODE] is connected.

[CODE]range.set_adjustment()[/CODE] does absolutely nothing if you pass it the 
adjustment that range is already using, regardless of whether you changed any of 
its fields or not. If you pass it a new Adjustment, it will unreference the old 
one if it exists (possibly destroying it), connect the appropriate signals to 
the new one, and call the private function [CODE]range.adjustment_changed()[/CODE], 
which will (or at least, is supposed to...) recalculate the size and/or position 
of the slider and redraw if necessary. As mentioned in the section on 
adjustments, if you wish to reuse the same Adjustment, when you modify its values 
directly, you should emit the "changed" signal on it, like this:

[CODE]
signal.emit_by_name ("changed");
[/CODE]




Key and Mouse bindings

All of the GTK range widgets react to mouse clicks in more or less the same way. 
Clicking button-1 in the trough will cause its adjustment's page_increment to be 
added or subtracted from its value, and the slider to be moved accordingly. 
Clicking mouse button-2 in the trough will jump the slider to the point at which 
the button was clicked. Clicking button-3 in the trough of a range or any button 
on a scrollbar's arrows will cause its adjustment's value to change by 
[CODE]step_increment[/CODE] at a time.

Scrollbars are not focusable, thus have no key bindings. The key bindings for 
the other range widgets (which are, of course, only active when the widget has 
focus) are do not differentiate between horizontal and vertical range widgets.

All range widgets can be operated with the left, right, up and down arrow keys, 
as well as with the [STRONG]Page Up[/STRONG] and [STRONG]Page Down[/STRONG] keys. 
The arrows move the slider up and down by [CODE]step_increment[/CODE], while 
[STRONG]Page Up[/STRONG] and [STRONG]Page Down[/STRONG] move it by 
[CODE]page_increment[/CODE].

The user can also move the slider all the way to one end or the other of the 
trough using the keyboard. This is done with the [STRONG]Home[/STRONG] and 
[STRONG]End[/STRONG] keys.




Range Widget Example

The example program ([CODE]chapter-08/code/01RangeWidgets.vala[CODE]) puts up a 
window with three range widgets all connected to the same adjustment, and a 
couple of controls for adjusting some of the parameters mentioned above and in 
the section on adjustments, so you can see how they affect the way these widgets 
work for the user. 

[CODE]

[/CODE]

The figure below illustrates the result of running the program:

[IMG file="chapter-04/screenshots/01RangeWidgets.png"]Range Widgets Example[/IMG]

You will notice that the program does not call [CODE]connect()[/CODE] method for 
the "delete-event", but only for the "destroy" signal. This will still perform 
the desired function, because an unhandled "delete-event" will result in a 
"destroy" signal being given to the window.
