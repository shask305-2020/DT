## Смена гридов после авторизации

```cs
public partial class MainWindow : Window, INotifyPropertyChanged
{
    public event PropertyChangedEventHandler PropertyChanged;

    private bool _PasswordVisibility = true;

    private bool PasswordVisibility {
        get {
            return _PasswordVisibility;
        }
        set {
            _PasswordVisibility = value;
            PasswordGrid.Visibility = PasswordVisibility ? Visibility.Visible : Visibility.Collapsed;
            ContentGrid.Visibility = PasswordVisibility ? Visibility.Collapsed : Visibility.Visible;
            if (PropertyChanged != null)
            {
                // обновляем не каждое поле по отдельности, а все окно
                PropertyChanged(this, new PropertyChangedEventArgs("Root"));
            }
        }
    }

    public MainWindow()
    {
        InitializeComponent();
        PasswordVisibility = true;
        DataContext = this;
    }

    private void LoginButton_Click(object sender, RoutedEventArgs e)
    {
        // тут нужно конечно навернуть проверку логина и пароля
        PasswordVisibility = !PasswordVisibility;
    }

    ...
```

## Класс с подключением к БД

```cs
class Core
{
    public static ekolesnikovEntities DB = new ekolesnikovEntities();
}
```    

