[Label Control](https://wpf-tutorial.com/basic-controls/the-label-control/)  
[Text Box](https://wpf-tutorial.com/basic-controls/the-textbox-control/,)  
[Button Control](https://wpf-tutorial.com/basic-controls/the-button-control/)  
[Dialog Boxes](https://learn.microsoft.com/en-us/dotnet/desktop/wpf/windows/dialog-boxes-overview?view=netdesktop-7.0&viewFallbackFrom=netdesktop-5.0)  
[Message Box](https://learn.microsoft.com/en-us/dotnet/api/system.windows.messagebox?view=windowsdesktop-8.0)
   

Window Properties:  
**Icon** ==- Allows you to define the icon of the window, which is usually shown in the upper left corner, to the left of the window title.==  
**ResizeMode** ==- This controls whether and how the end-user can resize your window. The default is CanResize, which allows the user to resize the window like any other window, either by using the maximize/minimize buttons or by dragging one of the edges. CanMinimize will allow the user to minimize the window, but not to maximize it or drag it bigger or smaller. NoResize is the strictest one, where the maximize and minimize buttons are removed and the window can't be dragged bigger or smaller.==  
**ShowInTaskbar** ==- The default is true, but if you set it to false, your window won't be represented in the Windows taskbar. Useful for non-primary windows or for applications that should minimize to the tray.==  
**SizeToContent** ==- Decide if the Window should resize itself to automatically fit its content. The default is Manual, which means that the window doesn't automatically resize. Other options are Width, Height and WidthAndHeight, and each of them will automatically adjust the window size horizontally, vertically or both.==  
**Topmost** ==- The default is false, but if set to true, your Window will stay on top of other windows unless minimized. Only useful for special situations.==  
**WindowStartupLocation** ==- Controls the initial position of your window. The default is Manual, which means that the window will be initially positioned according to the Top and Left properties of your window. Other options are CenterOwner, which will position the window in the center of it's owner window, and CenterScreen, which will position the window in the center of the screen.==  
**WindowState** ==- Controls the initial window state. It can be either Normal, Maximized or Minimized. The default is Normal, which is what you should use unless you want your window to start either maximized or minimized.==
 \> String deklaration:  

\<sys:String x:Key="strHelloWorld"\>Hello, world!\</sys:String\>
 
[Resurser](https://wpf-tutorial.com/wpf-application/resources/)  
Hvis resurser bliver deklareret i app.xaml, er de globale og tilgængelige overalt.  
StaticResource vs. DynamicResource  
In the examples so far, I have used the StaticResource markup extension to reference a resource. However, an alternative exists, in form of the DynamicResource.
 
**The main difference is that a static resource is resolved only once, which is at the point where the XAML is loaded. If the resource is then changed later on, this change will not be reflected where you have used the StaticResource.**
 
**A DynamicResource on the other hand, is resolved once it's actually needed, and then again if the resource changes.** Think of it as binding to a static value vs. binding to a function that monitors this value and sends it to you each time it's changed - it's not exactly how it works, but it should give you a better idea of when to use what. Dynamic resources also allows you to use resources which are not even there during design time, e.g. if you add them from Code-behind during the startup of the application.
 
Dialog boxes:
 ![Exported image](Exported%20image%2020251104230927-0.png)