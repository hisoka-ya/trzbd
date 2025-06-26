Конечно! Вот простейшие примеры кода для Microsoft SQL Server, включая триггер, представление, транзакцию и хранимую процедуру.

▎1. Пример триггера

Этот триггер будет срабатывать после вставки новой записи в таблицу Employees и будет записывать информацию об этом в таблицу AuditLog.

    CREATE TABLE Employees (
        EmployeeID INT PRIMARY KEY,
        Name NVARCHAR(100),
        Position NVARCHAR(100)
    );
    
    CREATE TABLE AuditLog (
        LogID INT IDENTITY(1,1) PRIMARY KEY,
        EmployeeID INT,
        Action NVARCHAR(50),
        ActionDate DATETIME DEFAULT GETDATE()
    );
    
    CREATE TRIGGER trg_AfterInsert_Employees
    ON Employees
    AFTER INSERT
    AS
    BEGIN
        INSERT INTO AuditLog (EmployeeID, Action)
        SELECT EmployeeID, 'Inserted'
        FROM inserted;
    END;


▎2. Пример представления

Это представление будет показывать имена сотрудников и их должности из таблицы Employees.

    CREATE VIEW vw_EmployeeDetails AS
    SELECT Name, Position
    FROM Employees;


▎3. Пример транзакции

Этот пример демонстрирует, как использовать транзакцию для обеспечения целостности данных при вставке в две таблицы.

    BEGIN TRANSACTION;
    
    BEGIN TRY
        INSERT INTO Employees (EmployeeID, Name, Position) VALUES (1, 'John Doe', 'Developer');
        INSERT INTO AuditLog (EmployeeID, Action) VALUES (1, 'Inserted');
    
    COMMIT TRANSACTION; -- Подтверждаем транзакцию
    END TRY
    BEGIN CATCH
        ROLLBACK TRANSACTION; -- Откатываем транзакцию в случае ошибки
        PRINT ERROR_MESSAGE();
    END CATCH;
    
    
    ▎4. Пример хранимой процедуры
    
    Эта хранимая процедура добавляет нового сотрудника в таблицу Employees.
    
    CREATE PROCEDURE sp_AddEmployee
        @EmployeeID INT,
        @Name NVARCHAR(100),
        @Position NVARCHAR(100)
    AS
    BEGIN
        INSERT INTO Employees (EmployeeID, Name, Position)
        VALUES (@EmployeeID, @Name, @Position);
    END;


▎Использование хранимой процедуры

Вы можете вызвать эту процедуру следующим образом:

    EXEC sp_AddEmployee @EmployeeID = 2, @Name = 'Jane Smith', @Position = 'Manager';


Эти примеры демонстрируют базовые операции в Microsoft SQL Server. Вы можете адаптировать их под свои нужды!
