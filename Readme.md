# WpfAppBar

Implementation of an `AppBar` in WPF based off of [Using Application Desktop Toolbars](https://msdn.microsoft.com/en-us/library/bb776821.aspx) and [Extend the Windows 95 Shell with Application Desktop Toolbars](https://www.microsoft.com/msj/archive/S274.aspx).

## Goals:

 - Allow dock to any side of the screen
 - Allow dock to a particular monitor
 - Allow resizing of the appbar
 - Handle screen layout changes and monitor disconnections
 - Handle <kbd>Win</kbd> + <kbd>Shift</kbd> + <kbd>Left</kbd> and attempts to minimize or move the window
 - Handle co-operation with other appbars (OneNote et al.)

 ## License

MIT License

## Example use

    <apb:AppBarWindow x:Class="WpfAppBarDemo.MainWindow" xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation" xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:apb="clr-namespace:WpfAppBar;assembly=WpfAppBar"
        DataContext="{Binding RelativeSource={RelativeSource Self}}" Title="MainWindow" 
        DockedWidthOrHeight="200">
        <Grid>
            <Button x:Name="btClose" Content="Close" HorizontalAlignment="Left" VerticalAlignment="Top" Width="75" Height="23" Margin="10,10,0,0" Click="btClose_Click"/>
            <ComboBox x:Name="cbMonitor" SelectedItem="{Binding Path=Monitor, Mode=TwoWay}" HorizontalAlignment="Left" VerticalAlignment="Top" Width="120" Margin="10,38,0,0"/>
            <ComboBox x:Name="cbEdge" SelectedItem="{Binding Path=DockMode, Mode=TwoWay}" HorizontalAlignment="Left" Margin="10,65,0,0" VerticalAlignment="Top" Width="120"/>

            <Thumb Width="5" HorizontalAlignment="Right" Background="Gray" x:Name="rzThumb" Cursor="SizeWE" DragCompleted="rzThumb_DragCompleted" />
        </Grid>
    </apb:AppBarWindow>

Codebehind:

    public partial class MainWindow
    {
        public MainWindow()
        {
            InitializeComponent();

            this.cbEdge.ItemsSource = new[]
            {
                AppBarDockMode.Left,
                AppBarDockMode.Right,
                AppBarDockMode.Top,
                AppBarDockMode.Bottom
            };
            this.cbMonitor.ItemsSource = MonitorInfo.GetAllMonitors();
        }

        private void btClose_Click(object sender, RoutedEventArgs e)
        {
            Close();
        }

        private void rzThumb_DragCompleted(object sender, DragCompletedEventArgs e)
        {
            this.DockedWidthOrHeight += (int)e.HorizontalChange;
        }
    }

## Screenshots

Changing docked position:

> [![AppBar docked to edges][1]][1]

Resizing with thumb:

> [![Resize][2]][2]

Cooperation with other appbars:

> [![Coordination][3]][3]

Clone [from GitHub](https://github.com/mgaffigan/WpfAppBar) if you want to use it.  The library itself is only three files, and can easily be dropped in a project.

  [1]: https://i.stack.imgur.com/f13P8.gif
  [2]: https://i.stack.imgur.com/ifgn8.gif
  [3]: https://i.stack.imgur.com/PiydR.gif