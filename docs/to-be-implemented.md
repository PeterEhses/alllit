### The Elements, Patterns and Templates shown here are yet to me implemented.

The following items may be stubs and are in no way a comprehensive list. This page should however give a rough overview of what needs to be ready for what to be implemented and what could be contributed next.

## Themeing

while all components currently available are set up to accept themeing in the future through either relying on css variables (preferred) or accepting HSV color for their functions, no central way of handling theme is implemented yet.

##### Themes to be implemented:

- Light (current theme, device light theme)
- Dark (device dark theme)
- extra dark (everything is absolute black except for necessary outlines, icons, text, etc.), for light spill situations
- extra dark muted (like extra dark but colors are additionally reduced to max 50% brightness), for light spill situations
  <br/>
  <img src="/img/farb varianten.png"
       style="max-width: 100%; max-height: 25vh;" />

## Elements

#### grid snap matrix

<img src="/img/Group 206.png"
     style="max-width: 100%; max-height: 25vh;" />
The grid snap matrix allows the user to select the points for transformable object snapping. i.e. the corner / center to which to align devices on the matrix

#### coordinate status floater

<img src="/img/COORDINATE STATUS.png"
     style="max-width: 100%; max-height: 25vh;" />
The coordinate status floater is part of the device side actions. It triggers locating the device on the canvas and shows the on canvas location of the device and extent on hover.

#### device status

<img src="/img/Group 242.png"
     style="max-width: 100%; max-height: 25vh;" />
The device status row is displayed at the bottom of device details. It shows the device status (online / offline / unknown / error) as well as an excerpt of the generated ArtNet messages. On hover, a floater like the coordinate status floater with the entire artnet message is displayed.

#### zoom scrollbar

<img src="/img/Zoom Scrollbar.png"
     style="max-width: 100%; max-height: 25vh;" />
Tthe zoom scrollbar allows for detailed scroll and zoom control. Through the two red handles, the displayed section can be controlled, by dragging or mouse scroll over, normal scroll occurs. Elements with zoom scrollbars should be zoomable through mouse scroll. This element has vertical and horizontal versions and should be displayed right / bottom on parent elements.

#### transform box

<img src="/img/Group 246.png"
     style="max-width: 100%; max-height: 25vh;" />
The transform box enables any child element to be transformed. It offers any of: rotation, scaling, proportional scaling, trapezoidal transform, move

#### code tab

<img src="/img/File_tab-3.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/File_tab.png"
     style="max-width: 100%; max-height: 25vh;" />
The code tab is the marker for any opened file in the code editor. It offers file manipulation options and can be rearranged in a given header section it's contained in.

#### combobox

<img src="/img/Group 127.png"
     style="max-width: 100%; max-height: 25vh;" />
The combobox allows for searching a list like through a classical dropdown, but offers a text input like behaviour for inputting new items into the list. If the text input matches part of any existing item, that item will be selected instead until the text input differs from the selected item.

#### text input

<img src="/img/Group 124.png"
     style="max-width: 100%; max-height: 25vh;" />
The text input offers a simple way to enter string data.

#### number input

<img src="/img/Group 137.png"
     style="max-width: 100%; max-height: 25vh;" />
The number input offers a simple way to input float or integer data. The input may only allow integer input or round at a given position.
A two arrow control at the end of the box allows the user to increment or decrement the value by clicking.

#### button

<img src="/img/Group 142.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/Group 147.png"
    style="max-width: 100%; max-height: 25vh;" />
Buttons form a basic interaction method. They can be assigned context dependent and come in a variety of visually weighted variants. They hold either text or Icons and may have dedicated disabled, enabled, hover, etc. states.

## Patterns

#### device side actions

<img src="/img/Group 243.png"
     style="max-width: 100%; max-height: 25vh;" />
The device side actions allow quick interaction with device instances in other locations including map view and real world.
All items have an on click and on hover action as well as icons for different status displays e.g. warning for not mapped, location icon for mapped, selected on click, red on selected, etc..

#### color history panel

<img src="/img/Group 108.png"
     style="max-width: 100%; max-height: 25vh;" />
The color history panel displays previously chosen colors from the color picker. The colors displays should be global meaning constant between color pickers throughout the application. The number stored should be fixed, the number displayed either user configurable or adapted to however much space is left for this component.
This component is part of the color picker if the color picker has a large enough area assigned.

#### window controls

<img src="/img/Group 244.png"
     style="max-width: 100%; max-height: 25vh;" />
The window controls replace the default system window controls. They should imitate the system window control behavior.

#### basic drawer

<img src="/img/drawer.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/drawer scroll.png"
    style="max-width: 100%; max-height: 25vh;" />
The basic drawer element supports either taking the first element as header or supplies its own header consisting of a foldout icon, name and drag handle.
Drawers can be arranged in the title bar by dragging on the handle and opened by clicking the top section.
If enough elements are in the drawer, it will switch from showing all to showing a defined number while displaying a scroll bar and a lower active corner with which to expand the panel to show more items at once.

#### DIP drawer

<img src="/img/DIP open.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/DIP.png"
    style="max-width: 100%; max-height: 25vh;" />
The dip panel is used where many controls need to fit in a narrow space. Currently, that's over any form input in the device template section, to cram in all the automation features needed.
Expanded it shows vertical switches with annotation, retracted it shows only the switches. The state can be changed by clicking the red triangle which also indicates state.

## Templates

#### template chip

<img src="/img/List Item.png"
     style="max-width: 100%; max-height: 25vh;" />
The template chip is a variant of the list chip reduced to information universal to all devices with a given template.

#### group chip

<img src="/img/gruppen folder.png"
     style="max-width: 100%; max-height: 25vh;" />
The group chip is a variant of the list chip that acts as a group header. It sets the color tag of all children and offers batch selection / editing options for the group equal to those provided on single list chips. the expand details icon expands or hides the list of children with an indent added to the children.

#### mapping chip

<img src="/img/Mapping Device Basic.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/Mapping Device.png"
    style="max-width: 100%; max-height: 25vh;" />
The mapping device is the representation of any given device on the pixel map. it is reduced in complexity compared to the list chip but adds transform controls including scale, rotate and lock.

#### panel drawer

<img src="/img/fenster add delete menu open.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/fenster add delete menu.png"
    style="max-width: 100%; max-height: 25vh;" />
The panel drawer holds any panels not currently displayed, shown as their tags. If a panel is deleted from the view or pulled into this drawer, it will automatically appear here. if a given tag is pulled out of this drawer, it will expand to the given tags panel and reflow in the view like all already active panels. Clicking the add icon will also add the panel to the active view.

#### view drawer

<img src="/img/Workspace Hint open.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/Workspace Hint.png"
    style="max-width: 100%; max-height: 25vh;" />
The view drawer shows all views. on click, the active view will change to the one selected. The uppermost view, also shown in closed state, is the active one.

#### terminal

<img src="/img/Terminal open.png"
     style="max-width: 100%; max-height: 25vh;" />
<img src="/img/Terminal closed.png"
    style="max-width: 100%; max-height: 25vh;" />
<img src="/img/Terminal error.png"
    style="max-width: 100%; max-height: 25vh;" />
The terminal has an open, a closed and a closed with error state.
It shows electron console output with standard highlighting and references to the relevant lines in the user code documents. On click, the code editor jumps to the given line / character.
Console messages are annotated with relevant icons and colored backgrounds.

#### device template section

<img src="/img/Group 254.png"
     style="max-width: 100%; max-height: 25vh;" />
The device template selection allows for generation and customization of device templates as well as manual registration of single devices.
It features automation boxes and inputs for all possible device settings.
The device tamplate displayed can be switched either through the device template list section or from the first combobox. If the template should be saved as new, the user should enter a new unique template name here.

#### auto discover section

<img src="/img/Group 255.png"
     style="max-width: 100%; max-height: 25vh;" />
The auto discover section features a list of all devices discovered in a given IP range through ArtNet ArtPoll, with best guesses about their device type and applicable template.
The user can select to ignore the suggested templates, select to add only a subgroup of the found devices and define the ip range to search.

#### template list section

<img src="/img/Group 113.png"
     style="max-width: 100%; max-height: 25vh;" />
The template list section displays all saved and preinstalled templates, highlights the active template (loaded in the device template view) and gives options to edit each template as well as show details through a dropdown similar to the device list.

#### code text editor

<img src="/img/Group 253.png"
     style="max-width: 100%; max-height: 50vh;" />
The code text editor has

- A side bar showing line numbers, highlighting lines corresponding to console messages, offering the possibility to hide logical sections of code (arrow symbol for open, primary color circle as marker for closed with a line number marked <kbd>//</kbd> to mark the hidden section of lines)
- A header showing rearrangable tabs for every file in the project folder, with a meatballs menu to show additional options (delete, rename, open in explorer), a button to create a new file and one to open a new project folder.
- The code to edit in the middle, with highlighting corresponding to console messages and hidden logical blocks as well as typical syntax / matching bracket / matching text highlighting, as well as a faint line in the indent of the beginning line if every logical block.

##### these parts should probably be broken out in at lease one pattern each, as well as elements as needed

## Panels (technically also templates)

#### title bar

<img src="/img/Title Bar.png"
     style="max-width: 100%; max-height: 50vh;" />
The title bar is a constant across all views and is always located across the entire top of the window. it holds all drawers and the window controls.

#### device templates panel

<img src="/img/Group 231.png"
     style="max-width: 100%; max-height: 50vh;" />

The Panel consists of three sections. A list of all device templates where new templates can be added. Template section contains device settings and the ColorPicker pattern. The settings are defined per fill-in, drop down or toggle switches.
The device name and IP is entered in a text input element in the template. To choose another template or name a new one you can use the combo box. To change the matrix coordinates or the number of Iterations use the number input element. If a property should be fixed, the lock icon can be used to set it. The Auto Discover Section helps find connected devices through searching an IP range.
Settings can be automated by using a locked value, automatically incrementing a number or number as string partial, filling in the reported IP or device ID or defined as not null, i.e. requiring the use to manually fill in the field on device registration. For not null fields and unlocked fields, the user can navigate from the first to the last through the <kbd>tab</kbd> key, ending on the add device button, to allow fast semi-manual setup.

#### device manager panel

<img src="/img/Group 238.png"
     style="max-width: 100%; max-height: 50vh;" />
A detailed list of the registered devices with all their properties. A search bar also allows you to filter through the list. To edit one or more devices you can select them with the checkmark icon.

#### code editor panel

<img src="/img/Group 239.png"
     style="max-width: 100%; max-height: 50vh;" />
The code editor consists of a header section, a code editing area and an expandable console overlay.
The header area gives controls over code tabs and file manipulation.

#### code preview panel

<img src="/img/Group 240.png"
     style="max-width: 100%; max-height: 50vh;" />
The code preview panel displays the canvas that the user code targets. it contains controls to execute and pause user code and reports status back through the editors terminal. It can be displayed as a separate window which in turn can be toggled fullscreen, through the windowed button in the header section.

#### mapping panel

<img src="/img/Group 230.png"
     style="max-width: 100%; max-height: 50vh;" />
The mapping panel consists of a header section with controls for- and the actual mapping view itself. the header controls change different aspects of how the mapping matrix is displayed (size, aspect) and how devices interact with it (rotation, snapping, transformation freedom, snapping point, should pixels be chosen by nearest neighbor (integer) or interpolated (float))

##### the pixel map will need additional definition that depends on the way it is logically implemented and rendered. hence no further components are broken out. this entry is a stub.

#### settings panel

<img src="/img/Group 252.png"
     style="max-width: 100%; max-height: 50vh;" />
The settings allow for user customization of the look and feel of the program as well as ease of use settings like choosing starting directory.
The theme can be chosen from any preinstalled one (see Themeing) (or possible user registered? a file based exchange format would be possible), accent and secondary accent color can be chosen through hue picker bars or entered as HEX values.
<br/>
The code editor gets extra customizability compared to the rest of the application in that a syntax theme and font family, style and size can be defined through user input.
