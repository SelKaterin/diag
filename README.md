# Процесс предоставления академического отпуска (AS-IS)

Диаграмма процесса (Mermaid):

```mermaid
flowchart TD
    %% Пул 1: Студент
    subgraph StudentPool ["Студент"]
        direction TB
        S([Старт])
        Apply[Подача заявления]
        Docs[Предоставление документов]
        Wait((Ожидание))
        Notify[Получение уведомления]
        E([Конец])
        S --> Apply
        Apply --> Docs
        Docs --> Wait
        Wait --> Notify
        Notify --> E
    end

    %% Пул 2: Деканат
    subgraph DeanPool ["Единый деканат"]
        direction TB
        Recv[Прием документов]
        Check[Проверка документов]
        Enter[Ввод данных в 1С Университет]
        CreateUni[Создание приказа в 1С Университет]
        DupDoc[Дублирование приказа в Документообороте]
        Send[Отправка на согласование]
        WaitAp((Ожидание согласования))
        Status[Получение статуса подписания]
        Update[Обновление данных в 1С]
        Post[Проведение приказа в 1С]
        Inform[Уведомление студента]
        EndD([Конец])

        Recv --> Check
        Check --> Enter
        Enter --> CreateUni
        CreateUni --> DupDoc
        DupDoc --> Send
        Send --> WaitAp
        WaitAp --> Status
        Status --> Update
        Update --> Post
        Post --> Inform
        Inform --> EndD
    end

    %% Пул 3: Согласующие
    subgraph ApprPool ["Согласующие подразделения"]
        direction TB
        Get[Получение на согласование]
        Decan[Согласование деканом]
        Acc[Согласование бухгалтерией]
        HR[Согласование отделом кадров]
        VP[Согласование проректором]
        Rect[Подписание ректором]
        Done([Завершено])

        Get --> Decan
        Decan --> Acc
        Acc --> HR
        HR --> VP
        VP --> Rect
        Rect --> Done
    end

    %% Межпуловые связи
    Docs -->|Передача| Recv
    Send -->|Запуск| Get
    WaitAp -.->|Ожидание статуса| Decan
    Rect -->|Статус готов| Status
    Inform -->|Уведомление| Notify
