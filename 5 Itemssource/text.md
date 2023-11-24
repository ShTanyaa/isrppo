# Элементы для вывода коллекций

Рассмотрим основные элементы для вывода коллекций:
- ItemsControl;
- ListBox;
- ListView;
- DataGrid;

Создадим приложение, демонстрируещее работу этих элементов.

В качестве примера выведем информацию о различных городах.


## 1. Добавление библиотеки Material Design

**(сами по себе используемые элементы этого не требуют, добавлено просто, чтобы показать, что так можно)**

Создадим новое приложение WPF. С помощью меню "Средства" -> "Диспетчер пакетов NuGet" -> "Управление пакетами NuGet для решения" загрузим пакет MaterialDesignThemes.

Для этого, в окне диспетчера пакетов, с помощью поиска найдем соответствующий пакет:
![](1.png)

Установим галочку для установки пакета, и нажмем "Установить".

В информации о пакете вы можете увидеть URL-адрес пакета, где можно ознакомиться с документацией.
![](2.png)

Из документации мы можем узнать, как его подключить:
https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit/wiki/Getting-Started

Сперва, добавим namespace в `app.xaml`:
```xml
...
xmlns:materialDesign="http://materialdesigninxaml.net/winfx/xaml/themes"
...
```

Далее, зададим словарь ресурсов и укажем базовые цвета:
```xml
<Application.Resources>
    <ResourceDictionary>
        <ResourceDictionary.MergedDictionaries>
            <materialDesign:BundledTheme BaseTheme="Light" PrimaryColor="DeepPurple" SecondaryColor="Cyan" />
            <ResourceDictionary Source="pack://application:,,,/MaterialDesignThemes.Wpf;component/Themes/MaterialDesignTheme.Defaults.xaml" />
        </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
</Application.Resources>
```

Готово! Мы можем использовать Material Design для разработки пользовательского интерфейса.


## 2. Верстка основого окна

Зададим верстку основого окна:
```xml
<Grid Margin="5">
    <Grid.RowDefinitions>
        <RowDefinition Height="50" />
        <RowDefinition Height="*" />
    </Grid.RowDefinitions>
    <StackPanel Orientation="Horizontal" VerticalAlignment="Center">
        <TextBlock Text="Используемая коллекция: " VerticalAlignment="Center" />
        <ComboBox Width="300" Margin="10 0 0 0">
        </ComboBox>
    </StackPanel>
    <Frame x:Name="mainFrame" Grid.Row="1" />
</Grid>
```

Самому окно зададим свойства `MinWidth` и `MinHeight`, чтобы задать его минимальные размеры (например, 700 и 450 соответственно).

## 3. Добавление городов

Добавим в проект папку `Models`. Разместим в ней класс:
```cs
public class City
{

    public string Name { get; set; } = string.Empty;
    public string Country { get; set; } = string.Empty;
    public int Population { get; set; }
    public decimal Price { get; set; }
    public string? Description { get; set; }
    public string? PhotoUrl { get; set; }
}
```

В коде `MainWindow` добавим список городов в виде приватного поля:
```cs
private List<City> cities = new List<City>
{
    new City
    {
        Country = "Россия",
        Name = "Екатеринбург",
        Description = "Екатеринбург является самым большим городом на Урале; центр Екатеринбургской агломерации. Один из 15 городов-миллионеров России.",
        Population = 1_500_000,
        Price = 25000,
        PhotoUrl = "https://thumb.tildacdn.com/tild6436-6264-4665-b565-346566393834/-/resize/834x/-/format/webp/shutterstock_1661579.jpg"
    },
    new City
    {
        Country = "Россия",
        Name = "Санкт-Петербург",
        Description = "Третий по численности населения город Европы, первый по численности населения город Европы, не являющийся столицей государства, и самый северный город с населением более миллиона человек; центр Санкт-Петербургской городской агломерации",
        Population = 5_600_044,
        Price = 35000,
        PhotoUrl = "https://avatars.mds.yandex.net/get-marketcms/1779479/img-fa8a7f10-039f-4543-b4ac-da59191126bd.jpeg/optimize"
    },
    new City
    {
        Country = "Россия",
        Name = "Москва",
        Description = "Столица России",
        Population = 99_999_999,
        Price = 25_000_000,
        PhotoUrl = "https://i.msmap.ru/original/331.jpg"
    },
    new City
    {
        Country = "Китай",
        Name = "Пекин",
        Description = "Пекин – столица Китая, история которой насчитывает три тысячелетия. Она славится как своими современными зданиями, так и древними архитектурными памятниками, среди которых величественный Запретный город – императорский дворец времен династий Мин и Цин.",
        Population = 21_500_000,
        Price = 110000,
        PhotoUrl = "https://ic.pics.livejournal.com/dergachev_va/58474394/2270770/2270770_original.jpg"
    },
    new City
    {
        Country = "Франция",
        Name = "Париж",
        Description = "Париж – один из главных европейских городов и мировой центр культуры, искусства, моды и гастрономии. В центральной части города, построенной в XIX веке, проходят широкие бульвары и протекает река Сена. Самые известные достопримечательности Парижа – Эйфелева башня и собор Парижской Богоматери в готическом стиле, возведенный в XII веке.",
        Population = 2_161_000,
        Price = 200000,
        PhotoUrl = "https://tripmydream.cc/travelhub/travel/blocks/12/8828/block_128828.jpg?v1"
    }
};
```

Данный список городов будем использовать как истоник данных.

## 4. DataGrid

Выполним вывод информацию о городах в элемент `DataGrid`. `DataGrid` - "сетка" для вывода значений. Хорошо подходит для вывода большого количества данных. Поддерживает редактирование.

Сперва создадим в проекте папку `Views`. Создадим в ней страницу WPF с название `DataGridPage`.

Зададим следующую разметку:
```xml
<DockPanel>
    <TextBlock DockPanel.Dock="Top" Text="Список городов (DataGrid)" Style="{DynamicResource MaterialDesignCaptionTextBlock}" />
    <TextBlock DockPanel.Dock="Bottom" Text="Всего наименований: 555" />
    <DataGrid>

    </DataGrid>
</DockPanel>
```

В `MainWindow` добавим еще одно поле:
```cs
private Page[] pages;
```

И в коде конструктора `MainWindow` добавим:
```cs
public MainWindow()
{
    InitializeComponent();
    pages = new Page[]
    {
        new DataGridPage()
    };
}
```

Изменим код класса `DataGridPage`, добавив параметр конструктора:
```cs
private readonly List<City> cities;

public DataGridPage(List<City> cities)
{
    InitializeComponent();
    this.cities = cities;
}
```

В `MainWindow` передадим параметром список:
```cs
pages = new Page[]
{
    new DataGridPage(cities)
};
```

Далее, в коде разметки `MainWindow` дадим имя для `ComboBox`:
```xml
<ComboBox Width="300" x:Name="pagesComboBox">
</ComboBox>
```

И заполним его элементами, указав значение для `ItemsSource`:
```cs
public MainWindow()
{
    InitializeComponent();
    pages = new Page[]
    {
        new DataGridPage(cities)
    };
    pagesComboBox.ItemsSource = pages;
    pagesComboBox.DisplayMemberPath = "Title";
}
```

Изменим `Title` в коде разметки `DataGridPage`:
```xml
Title="Список городов - DataGrid"
```

Запустим приложение и проверим работоспособность:
![](3.png)

Реализуем переход. Добавим для `pagesComboBox` обработчик события:
```xml
<ComboBox Width="300" x:Name="pagesComboBox" SelectionChanged="SelectWindow" />
```

```cs
private void SelectWindow(object sender, SelectionChangedEventArgs e)
{
    Page? page = pagesComboBox.SelectedItem as Page;
    mainFrame.Navigate(page);
}
```

Запустим и проверим переход на страницу.
![](4.png)

В коде разметки `DataGridPage` дадим название для `DataGrid`:
```xml
<DataGrid x:Name="citiesDataGrid">

</DataGrid>
```

И зададим для него `ItemsSource`:
```cs
public DataGridPage(List<City> cities)
{
    InitializeComponent();
    this.cities = cities;
    citiesDataGrid.ItemsSource = cities;
}
```

Запустим и получим следующий результат:
![](5.png)

Отметим, что:
- можно редактировать значения;
- столбцы были сформированы автоматически.

Изменим поведение `DataGrid`. Зададим следующее:
```xml
<DataGrid x:Name="citiesDataGrid" AutoGenerateColumns="False"  IsReadOnly="True">

</DataGrid>
```
`AutoGenerateColumns` зададим в `false`, что отключит автоматическую генерацию колонок, а `IsReadOnly` сделает сетку неизменяемой.

Опишем колонки вручную. Используем `DataGridTextColumn` для простого текста и `DataGridTemplateColumn` для более сложного содержимого.

Добавим пару колонок:
```xml
<DataGrid x:Name="citiesDataGrid" AutoGenerateColumns="False"  IsReadOnly="True">
    <DataGrid.Columns>
        <DataGridTextColumn Header="Название" Binding="{Binding Name}" />
        <DataGridTextColumn Header="Цена" Binding="{Binding Price, StringFormat={}{0} рублей}" />
    </DataGrid.Columns>
</DataGrid>
```

`Header` устанавливает заголовок колонки. `Binding` указывает свойство для привязки. Идея следующая: если задан `ItemsSource` как коллекция некоторых элементов `X` (например, `List<X>`, `X[]` или какой-то другой тип), то контекстом привязки для одного элемента будет значение типа `X`. В данном примере мы привязали `List<City>`, значит для одного элемента контекстом будет конкретный город типа `City`. **Его свойства мы можем прописать внутри `{Binding}`**.

`StringFormat={}{0} рублей}` позволяет задать форматированную строку. Началом строки является `{}`, подстрока `{0}` - место, куда будет подставлено значение, взятое из привязки. Так, при запуске мы увидем не только число (скажем, 25000), но и слово "рублей".

Запустим приложение, чтобы увидеть результат:
![](6.png)

Добавим другие поля:
```xml
<DataGrid.Columns>
    <DataGridTemplateColumn Header="Фотография">
        <DataGridTemplateColumn.CellTemplate>
            <DataTemplate>
                <Image Source="{Binding PhotoUrl}" Height="50" />
            </DataTemplate>
        </DataGridTemplateColumn.CellTemplate>
    </DataGridTemplateColumn>
    <DataGridTextColumn Header="Название" Binding="{Binding Name}" />
    <DataGridTextColumn Header="Страна" Binding="{Binding Country}" />
    <DataGridTextColumn Header="Население" Binding="{Binding Population}" />
    <DataGridTextColumn Header="Цена" Binding="{Binding Price, StringFormat={}{0} рублей}" />
</DataGrid.Columns>
```

С помощью `DataGridTemplateColumn` мы можем создать ячейку со сложной разметкой. В данном случае мы прописали внутри элемент `Image` для вывода изображения.

Добавим еще одну ячейку с кнопками, чтобы показать такую возможность. Сперва подключим пространство имен Material Design:
```xml
xmlns:md="http://materialdesigninxaml.net/winfx/xaml/themes"
```

Далее, добавим новую ячейку:
```xml
<DataGrid.Columns>
    <DataGridTemplateColumn Header="Фотография">
        <DataGridTemplateColumn.CellTemplate>
            <DataTemplate>
                <Image Source="{Binding PhotoUrl}" Height="50" />
            </DataTemplate>
        </DataGridTemplateColumn.CellTemplate>
    </DataGridTemplateColumn>
    <DataGridTextColumn Header="Название" Binding="{Binding Name}" />
    <DataGridTextColumn Header="Страна" Binding="{Binding Country}" />
    <DataGridTextColumn Header="Население" Binding="{Binding Population}" />
    <DataGridTextColumn Header="Цена" Binding="{Binding Price, StringFormat={}{0} рублей}" />
    <DataGridTemplateColumn>
        <DataGridTemplateColumn.CellTemplate>
            <DataTemplate>
                <StackPanel>
                    <Button HorizontalContentAlignment="Left" 
                            Style="{DynamicResource MaterialDesignFlatButton}">
                        <TextBlock>
                            <md:PackIcon Kind="Edit" /> Редактировать
                        </TextBlock>
                    </Button>
                    <Button HorizontalContentAlignment="Left" 
                            Style="{DynamicResource MaterialDesignFlatButton}">
                        <TextBlock>
                            <md:PackIcon Kind="Delete" /> Удалить
                        </TextBlock>
                    </Button>
                </StackPanel>
            </DataTemplate>
        </DataGridTemplateColumn.CellTemplate>
    </DataGridTemplateColumn>
</DataGrid.Columns>
```

Результат будет выглядеть так:
![](7.png)

Наконец, выполним привязку для "Всего наименований":
```xml
<TextBlock DockPanel.Dock="Bottom">
    Всего наименований: <Run Text="{Binding ElementName=citiesDataGrid, Path=ItemsSource.Count,Mode=OneWay}" />
</TextBlock>
```

И немного изменим стиль заголовка:
```xml
<TextBlock DockPanel.Dock="Top" Text="Список городов (DataGrid)" Margin="0 0 0 20" Style="{DynamicResource MaterialDesignBody1TextBlock}" />
```

Выполните все действия и проверьте работоспособность.


## 5. ListBox

`ListBox` - элемент для вывода коллекции списком. Продемонстрируем его работу, осуществив вывод списка городов на экран.
 Создадим страницу `ListBoxPage` в папке `Views`.
 Переопределим для нее конструктор и добавим скрытое поле:
 ```cs
 private readonly List<City> cities;

public ListBoxPage(List<City> cities)
{
    InitializeComponent();
    this.cities = cities;
}
```

Добавим ее в массив `Page[]`, чтобы реализовать переход (в коде `MainWindow`):
```cs
pages = new Page[]
{
    new DataGridPage(cities),
    new ListBoxPage(cities)
};
```

На самой странице добавьте
```xml
<DockPanel>
    <TextBlock DockPanel.Dock="Top" Text="Список городов (ListBox)" Margin="0 0 0 20" Style="{DynamicResource MaterialDesignBody1TextBlock}" />
    <TextBlock DockPanel.Dock="Bottom">
        Всего наименований: 555
    </TextBlock>
    <ListBox x:Name="citiesListBox">

    </ListBox>
</DockPanel>
```

Проверьте переход:
![](8.png)

Если все хорошо, то зададим свойство `ItemsSource`:
```cs
public partial class ListBoxPage : Page
{
    private readonly List<City> cities;

    public ListBoxPage(List<City> cities)
    {
        InitializeComponent();
        this.cities = cities;
        citiesListBox.ItemsSource = cities;
    }
}
```

Сразу выполним привязку для количества наименований:
```xml
<TextBlock DockPanel.Dock="Bottom">
    Всего наименований: <Run Text="{Binding ElementName=citiesListBox, Path=ItemsSource.Count,Mode=OneWay}" />
</TextBlock>
```

Полученный результат:
![](9.png)

Изменим макет одного элемента. Сперва зададим `DataTemplate`
```xml
<ListBox x:Name="citiesListBox">
    <ListBox.ItemTemplate>
        <DataTemplate>

        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Далее, опишем макет для каждого элемента:
```xml
<ListBox x:Name="citiesListBox">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="150" />
                    <ColumnDefinition Width="*"  />
                    <ColumnDefinition Width="250" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding PhotoUrl}" />
                <TextBlock Grid.Column="1" FontSize="16" Margin="10 0">
                    <Run FontSize="20" FontWeight="Bold" Text="{Binding Name}" /> - 
                    <Run FontSize="20" FontWeight="Bold" Text="{Binding Country}" />
                    <LineBreak />
                    Население: <Run Text="{Binding Population}" />
                    <LineBreak />
                    <Run Text="{Binding Description}" />
                </TextBlock>
                <Button Grid.Column="2">
                    <TextBlock>
                        Купить билет за <Run Text="{Binding Price}" />
                    </TextBlock>
                </Button>
            </Grid>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Получим следующий результат:
![](10.png)

Зададим выравнивание для содержимого `ListBox` с помощью свойства `HorizontalContentAlignment`:
```xml
<ListBox x:Name="citiesListBox" HorizontalContentAlignment="Stretch">
```
![](11.png)

Зададим перенос текста с помощью свойства `TextWrapping` (для `TextBlock` посередине):
```xml
<TextBlock Grid.Column="1" FontSize="16" Margin="10 0" TextWrapping="Wrap">
    <Run FontSize="20" FontWeight="Bold" Text="{Binding Name}" /> - 
    <Run FontSize="20" FontWeight="Bold" Text="{Binding Country}" />
    <LineBreak />
    Население: <Run Text="{Binding Population}" />
    <LineBreak />
    <Run Text="{Binding Description}" />
</TextBlock>
```

Получим следующий результат:
![](12.png)

## 6. ListView

Вы можете работать с `ListView` таким же образом, как и с `ListBox`. Основные отличия между ними следующие:
- ListView поддерживает множественное выделение;
- для ListView мы можем проделать все то, же что и в предыдущем пункте, однако, для него существует немного другой способ привязки, который будет продемонстрирован далее.

Создайте страницу `ListViewPage`. Измените ее конструктор, добавив параметром `cities`. Добавьте `ListViewPage` в код `MainWindow`, чтобы реализовать переход.

Добавьте разметку:
```xml
<DockPanel>
    <TextBlock DockPanel.Dock="Top" Text="Список городов (ListView)" Margin="0 0 0 20" Style="{DynamicResource MaterialDesignBody1TextBlock}" />
    <TextBlock DockPanel.Dock="Bottom">
        Всего наименований: <Run Text="{Binding ElementName=citiesListView, Path=ItemsSource.Count,Mode=OneWay}" />
    </TextBlock>
    <ListView x:Name="citiesListView">

    </ListView>
</DockPanel>
```

И задайте `ItemsSource`:
```cs
citiesListView.ItemsSource = cities;
```

Полученный результат:
![](13.png)

Выполним привязку и зададим шаблон элементов.

Для этого напишем код:
```xml
<ListView x:Name="citiesListView">
    <ListView.View>
        <GridView>
            <GridViewColumn Header="Изображение">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <Image Source="{Binding PhotoUrl}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="Название">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Name}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="Страна">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Country}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="Население">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Population}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
        </GridView>
    </ListView.View>
</ListView>
```

Основной контейнер (свойство) для колонок - `ListView.View`. В нем мы задаем `GridView`. Внутри мы описываем массив колонок. Для каждой колонки мы можем задать `Header` и `CellTemplate`. Также, как и в предыдущих случаях, вы можете создавать сложную разметку внутри `DataTemplate`.
Например, сделаем ячейку с общей информацией о городе:
```xml
<ListView x:Name="citiesListView" >
    <ListView.View>
        <GridView>
            <GridViewColumn Header="Изображение">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <Image Source="{Binding PhotoUrl}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="Информация" Width="800">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBlock FontSize="16" TextWrapping="Wrap">
                            <Run FontSize="20" FontWeight="Bold" Text="{Binding Name}" />
                            <LineBreak />
                            Страна: <Run Text="{Binding Country}" />
                            <LineBreak />
                            Население <Run Text="{Binding Population}" />
                            <LineBreak />
                            <LineBreak />
                            <Run FontSize="14" Text="{Binding Description}" />
                        </TextBlock>
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
            <GridViewColumn Header="Цена">
                <GridViewColumn.CellTemplate>
                    <DataTemplate>
                        <TextBlock Text="{Binding Price}" />
                    </DataTemplate>
                </GridViewColumn.CellTemplate>
            </GridViewColumn>
        </GridView>
    </ListView.View>
</ListView>
```

Кстати, обратите внимание на еще одну особенность, пользователь может перетаскивать или изменять размеры столбцов.

Последний штрих - задание стиля для заголовков. Напишите внутри узла `GridView` (в самом начале)
```xml
<GridView.ColumnHeaderContainerStyle>
    <Style BasedOn="{StaticResource MaterialDesignToolForegroundButton}" TargetType="GridViewColumnHeader">
        <Setter Property="HorizontalContentAlignment" Value="Center" />
    </Style>
</GridView.ColumnHeaderContainerStyle>
```

Полученный результат:
![](14.png)

## 7. ItemsControl

`ItemsControl` похож на `ListBox` или `ListView`, но в нем нельзя выделять элементы. Подходит, если что-то нужно просто вывести списком на экран.

По аналогии с предыдущими пунктами:
- создайте страницу `ItemsControlPage`;
- разместите на ней заголовок и информацию о количестве записей, а также элемент `ItemsControl`;
- реализуйте переход на страницу `ItemsControlPage`;
- передайте через конструктор список городов;
- установите свойство `ItemsSource` присвоив ему список городов.

Если все было сделано правильно, то вы получите результат:
![](15.png)

Добавим в код разметки пространство имен `MaterialDesign`:
```xml
...
xmlns:md="http://materialdesigninxaml.net/winfx/xaml/themes"
...
```

И напишем разметку для элемента:
```xml
<ItemsControl x:Name="citiesItemsControl" >
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <md:Card Margin="5">
                <TextBlock>
                    <Run Text="{Binding Name}" /> - <Run Text="{Binding Country}" />
                </TextBlock>
            </md:Card>
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```

Полученный результат:
![](16.png)

Как вы видите, элеметы по умолчанию идут как в `StackPanel`. Мы можем переопределить это поведение (то же справедливо и для `ListBox`, и для `ListView`) задав свойство `ItemsPanel`:
```xml
<ItemsControl x:Name="citiesItemsControl" >
    <ItemsControl.ItemsPanel>
        <ItemsPanelTemplate>
            <WrapPanel />
        </ItemsPanelTemplate>
    </ItemsControl.ItemsPanel>
    <ItemsControl.ItemTemplate>
    ...
    
    
```

Мы задали WrapPanel, что заставит элементы идти в ряд и позволит нам реализовать "плитку". Зададим шаблон для одного элемента:
```xml
<ItemsControl.ItemTemplate>
    <DataTemplate>
        <md:Card Margin="10" Padding="5" Width="300" Height="400">
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="200" />
                    <RowDefinition />
                    <RowDefinition Height="50" />
                </Grid.RowDefinitions>
                <Image Source="{Binding PhotoUrl}" />
                <TextBlock Grid.Row="1" FontSize="20" FontWeight="Bold" TextWrapping="Wrap">
                    <Run Text="{Binding Name}" /> - <Run Text="{Binding Country}" />
                    <LineBreak />
                    <Run Text="{Binding Description}" FontWeight="Regular"  FontSize="12" />
                </TextBlock>
                <Button Grid.Row="2">
                    <TextBlock>Купить за <Run Text="{Binding Price}"/> рублей</TextBlock>
                </Button>
            </Grid>
        </md:Card>
    </DataTemplate>
</ItemsControl.ItemTemplate>
```

Полученный результат:
![](17.png)

Под конец добавим полосу прокрутки, разместив `ItemsControl` внутри специального элемента `ScrollViewer`:
```xml
<ScrollViewer>
    <ItemsControl x:Name="citiesItemsControl" >
			...
    </ItemsControl>
</ScrollViewer>
```

Итоговый результат:
![](18.png)

## Самостоятельная работа

Разработайте самостоятельно приложение, которое бы выводило информацию о 5-6 наименованиях (например о товарах, сотрудниках или компьютерной технике). Вы можете выбрать предметную область самостоятельно (конечно, действуя в рамках приличия). Обязательным будет вывод изображения (вы можете скачать их, как в работе про мечи и топоры, или указать url изображений) и как минимум 3 других текстовых полей. Выберите для вывод один из элементов: `DataGrid`, `ListBox`, `ListView` или `ItemsControl`. Опционально вы можете использовать библиотеку "Material Design Themes". 