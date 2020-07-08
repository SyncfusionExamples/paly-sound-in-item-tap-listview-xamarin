# How to make sound on tapping an item in Xamarin.Forms ListView (SfListView)

Xamarin.Forms [SfListView](https://help.syncfusion.com/xamarin/listview/overview) supports to use common API to load audio data from a shared location across UWP, iOS & Android with the help of [SimpleAudioPlayer](https://github.com/adrianstevens/Xamarin-Plugins/tree/master/SimpleAudioPlayer) plugin. 

You can also refer the following article.

https://www.syncfusion.com/kb/11687/how-to-make-sound-on-tapping-an-item-in-xamarin-forms-listview-sflistview

You can refer the following steps to achieve the requirement.

**STEP 1:** Install the [Xam.Plugin.SimpleAudioPlayer](http://www.nuget.org/packages/Xam.Plugin.SimpleAudioPlayer) Nuget package in the shared code project.

**STEP 2:** Place the audio file in the shared code project and set the [Build action](https://docs.microsoft.com/en-us/visualstudio/ide/build-actions?view=vs-2019) as follows,

| Project | Location  | Build action   |
|---------|-----------|----------------|
| Android | Assets    | AndroidAsset   |
| iOS     | Resources | BundleResource |
| UWP     | Assets    | Content        |

You can also refer to the link below to use custom fonts in Xamarin.Forms.
https://devblogs.microsoft.com/xamarin/adding-sound-xamarin-forms-app/

**XAML**

Bind the [TapCommand](https://help.syncfusion.com/cr/cref_files/xamarin/Syncfusion.SfListView.XForms~Syncfusion.ListView.XForms.SfListView~TapCommand.html) of the SfListView.
``` xml
<syncfusion:SfListView x:Name="listView" ItemSize="60" ItemsSource="{Binding ContactsInfo}" TapCommand="{Binding TappedCommand}">
    <syncfusion:SfListView.ItemTemplate >
        <DataTemplate>
            <Grid x:Name="grid">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding ContactImage}" VerticalOptions="Center" HorizontalOptions="Center" HeightRequest="50" WidthRequest="50"/>
                <Grid Grid.Column="1" RowSpacing="1" Padding="10,0,0,0" VerticalOptions="Center">
                    <Label LineBreakMode="NoWrap" TextColor="#474747" Text="{Binding ContactName}"/>
                    <Label Grid.Row="1" Grid.Column="0" TextColor="#474747" LineBreakMode="NoWrap" Text="{Binding ContactNumber}"/>
                </Grid>
            </Grid>
        </DataTemplate>
    </syncfusion:SfListView.ItemTemplate>
</syncfusion:SfListView>
```

**C#**

In the ViewModel class, loaded the audio file. Play the audio by calling **Play** method in the command execution method.
``` c#
public class ContactsViewModel : INotifyPropertyChanged
{
    ISimpleAudioPlayer Player;
    public ObservableCollection<Contacts> ContactsInfo { get; set; }
    public Command TappedCommand { get; set; }

    public ContactsViewModel()
    {
        ContactsInfo = new ObservableCollection<Contacts>();
        TappedCommand = new Command(OnTapped);

        Player = CrossSimpleAudioPlayer.Current;
        Player.Load("TapSound.wav");

        GenerateInfo();
    }

    private void OnTapped()
    {
        Player.Play();
    }
}
```

