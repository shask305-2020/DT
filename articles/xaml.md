## Верстка двух параллельных гридов

```xml
        Name="Root">
        <!-- окну даем имя -->
    <Grid>
        <Grid.ColumnDefinitions>
            <!-- создаем две колонки с шириной "auto" -->
            <ColumnDefinition Width="auto"/>
            <ColumnDefinition Width="auto"/>
        </Grid.ColumnDefinitions>

        <!-- и создаем два грида, один для верстки формы с логином/паролем -->
        <Grid 
            Name="PasswordGrid"
            Visibility="Visible"
            Width="{Binding ElementName=Root, Path=Width}">
            
            ...
            
        </Grid>

        <!-- второй для основной верстки, он пока скрыт: Visibility="Collapsed"  -->
        <Grid 
            Name="ContentGrid"
            Grid.Column="1"
            Visibility="Collapsed"  
            Width="{Binding ElementName=Root, Path=Width}">
            
            ...
            
        </Grid>
    </Grid>
</Window>
```

