# <a name="add-a-model-to-an-aspnet-core-mvc-app"></a>Добавление модели в приложение MVC ASP.NET Core

Авторы: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson) и [Том Дайкстра](https://github.com/tdykstra) (Tom Dykstra)

В этом разделе мы добавим некоторые классы для управления фильмами в базе данных. Эти классы будут представлять уровень **м**одели в приложении **M**VC.

Эти классы используются в [Entity Framework Core](/ef/core) (EF Core) для работы с базой данных. EF Core —это платформа объектно реляционного сопоставления (ORM), которая позволяет упростить необходимый код доступа к данным. [EF Core поддерживает множество систем баз данных](/ef/core/providers/).

Создаваемые вами классы моделей называются классами POCO (от "plain old CLR objects" — старые добрые объекты CLR), так как они не зависят от EF Core. Эти классы просто определяют свойства данных, которые будут храниться в базе данных.

Работая с этим учебником, вы напишете классы моделей, а затем EF Core создаст базу данных. Другой вариант, который здесь не рассматривается, состоит в создании классов моделей из уже существующей базы данных. Дополнительные сведения об этом подходе см. в разделе [ASP.NET Core — существующая база данных](/ef/core/get-started/aspnetcore/existing-db).

## <a name="add-a-data-model-class"></a>Добавление класса модели данных
