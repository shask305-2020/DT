# Примеры SQL-запросов

1. Создание клиента (по ТЗ ФИО могут быть не указаны). Для агента аналогично, но для ФИО добавить `NOT NULL`, т.к. для него это обязательные поля.

    ```sql
    CREATE TABLE Clients (
        Id int IDENTITY(1,1) NOT NULL, 
            CONSTRAINT PK_Clients_Id PRIMARY KEY (Id),
        FirstName nvarchar(100),
        MiddleName nvarchar(100),
        LastName nvarchar(100),
        Phone int,
        EMail varchar(100)
    )
    ```

    Название первичного ключа должно быть уникальным (это отдельная сущность в БД), правила наименования простые: `PK_НазваниеТаблицы_НазваниеПоля`

2. Создание таблицы "предложения" (Offers) с внешними ключами

    ```sql
    -- клиент, риэлтор, объект недвижимости, цена.
    CREATE TABLE Offers (
        Id int IDENTITY(1,1) NOT NULL,
            CONSTRAINT PK_Offers_Id PRIMARY KEY (Id),
        ClientId int NOT NULL,
            CONSTRAINT FK_Offers_Clients FOREIGN KEY (ClientId)
                REFERENCES Clients (Id)
                ON DELETE CASCADE
                ON UPDATE CASCADE,
        AgentId int NOT NULL,
            CONSTRAINT FK_Offers_Agents FOREIGN KEY (AgentId)
                REFERENCES Agents (Id)
                ON DELETE CASCADE
                ON UPDATE CASCADE,
        ApartmentId int NOT NULL,
            CONSTRAINT FK_Offers_Apartments FOREIGN KEY (ApartmentId)
                REFERENCES Apartments (Id)
                ON DELETE CASCADE
                ON UPDATE CASCADE,
        Price int
    )
    ```

    Естественно создавать эту таблицу можно только после того, как созданы таблицы **Clients**, **Agents** и **Apartments**

    Правила наименования внешнего ключа: `PK_НазваниеТекущейТаблицы_НазваниеВнешнейТаблицы`

3. Заполнение словаря городов

    ```sql
    insert into Cities ([Name]) 
        select distinct Address_City from apartment_a_import
    ```

4. Перенос данных из временной таблицы в основную (с учетом словарей)

    ```sql
    INSERT INTO Apartments (AddressCityId, AddressStreetId, AddressHouse, AddressNumber,... дельше мне лень писать )
        SELECT s.id, st.id, ai.Address_House,... дальше тоже лень
        FROM apartment_a_import ai, 
             Sities s,
             Streets st
        WHERE s.[Name]=ai.Address_City AND st.Name=ai.Address_Street
    ```

**Типы данных:**

* int - целое
* nvarchar(255) - строка в UNICODE (в скобках максимальная длина данных в поле)
* varchar(255) - строка в ANSI (в скобках максимальная длина данных в поле)
* float - число с плавающей запятой
* decimal(11,2) - денежный формат
* date - дата
* datetime - дата со временем