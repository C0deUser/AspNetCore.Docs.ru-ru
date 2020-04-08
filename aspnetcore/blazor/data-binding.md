---
title: Привязка к данным в ASP.NET Core Blazor
author: guardrex
description: Сведения о функциях привязки данных для компонентов и элементов модели DOM в приложениях Blazor.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2020
no-loc:
- Blazor
- SignalR
uid: blazor/data-binding
ms.openlocfilehash: a7b3730dad48b5bbb6134dab181051da4e3651b4
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2020
ms.locfileid: "80320948"
---
# <a name="aspnet-core-opno-locblazor-data-binding"></a>Привязка к данным в ASP.NET Core Blazor

Авторы: [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Дэниэл Рот](https://github.com/danroth27) (Daniel Roth)

Компоненты Razor реализуют функции привязки данных для поля, свойства или значения выражения Razor с помощью атрибута HTML-элемента [`@bind`](xref:mvc/views/razor#bind).

В следующем примере показана привязка свойства `CurrentValue` к значению текстового поля:

```razor
<input @bind="CurrentValue" />

@code {
    private string CurrentValue { get; set; }
}
```

Когда текстовое поле теряет фокус, значение свойства обновляется.

Текстовое поле обновляется в пользовательском интерфейсе только при отрисовке компонента, а не в ответ на изменение значения свойства. Так как компоненты отрисовываются после выполнения кода обработчика событий, изменения свойств *обычно* отражаются в пользовательском интерфейсе сразу после активации обработчика событий.

Использование атрибута `@bind` со свойством `CurrentValue` (`<input @bind="CurrentValue" />`) фактически эквивалентно следующему:

```razor
<input value="@CurrentValue"
    @onchange="@((ChangeEventArgs __e) => CurrentValue = 
        __e.Value.ToString())" />
        
@code {
    private string CurrentValue { get; set; }
}
```

Когда компонент отрисовывается, значение `value` элемента input берется из свойства `CurrentValue`. Когда пользователь вводит данные в текстовом поле, а затем переводит фокус, инициируется событие `onchange`, и свойству `CurrentValue` присваивается измененное значение. На практике код обычно сложнее, так как атрибут `@bind` применяется в случаях, когда выполняется преобразование типов. Как правило, `@bind` связывает текущее значение выражения с атрибутом `value` и обрабатывает изменения с помощью зарегистрированного обработчика.

Чтобы привязать свойство или поле к другим событиям, можно использовать атрибут `@bind:event` с параметром `event`. В следующем примере свойство `CurrentValue` привязывается к событию `oninput`:

```razor
<input @bind="CurrentValue" @bind:event="oninput" />

@code {
    private string CurrentValue { get; set; }
}
```

В отличие от события `onchange`, которое происходит, когда элемент теряет фокус, событие `oninput` происходит при изменении значения в текстовом поле.

Для привязки атрибутов элементов, отличных от `@bind-{ATTRIBUTE}`, используйте `@bind-{ATTRIBUTE}:event` с синтаксисом `value`. В следующем примере стиль абзаца обновляется при изменении значения `_paragraphStyle`:

```razor
@page "/binding-example"

<p>
    <input type="text" @bind="_paragraphStyle" />
</p>

<p @bind-style="_paragraphStyle" @bind-style:event="onchange">
    Blazorify the app!
</p>

@code {
    private string _paragraphStyle = "color:red";
}
```

Привязка атрибута учитывает регистр. Например, `@bind` допустимо использовать, а `@Bind` нет.

## <a name="unparsable-values"></a>Значения, не поддающиеся анализу

Когда пользователь присваивает не поддающееся анализу значение элементу, привязанному к данным, при активации события привязки автоматически восстанавливается предыдущее значение.

Рассмотрим следующий сценарий.

* Элемент `<input>` привязан к типу `int` с начальным значением `123`:

  ```razor
  <input @bind="MyProperty" />

  @code {
      [Parameter]
      public int MyProperty { get; set; } = 123;
  }
  ```
* Пользователь изменяет значение элемента на `123.45` на странице и переводит фокус.

В приведенном выше сценарии восстанавливается значение элемента `123`. Когда значение `123.45` отклоняется и вместо него используется исходное значение `123`, пользователь понимает, что его значение не было принято.

По умолчанию привязка применяется к событию `onchange` элемента (`@bind="{PROPERTY OR FIELD}"`). Чтобы выполнить привязку к другому событию, используйте `@bind="{PROPERTY OR FIELD}" @bind:event={EVENT}`. Для события `oninput` (`@bind:event="oninput"`) исходное значение восстанавливается после любого нажатия клавиши, которое приводит к вводу не поддающегося анализу значения. При привязке типа `oninput` к событию `int` пользователь не может ввести символ `.`. Символ `.` немедленно удаляется, так что пользователь сразу понимает, что допустимы только целые числа. В некоторых ситуациях восстанавливать исходное значение при возникновении события `oninput` нежелательно, например, когда пользователю следует дать возможность самому удалять не поддающиеся анализу значения `<input>`. Ниже представлены возможные альтернативы.

* Не используйте событие `oninput`. Используйте вместо этого событие `onchange` по умолчанию (укажите просто `@bind="{PROPERTY OR FIELD}"`). В этом случае недопустимое значение не будет меняться на исходное, пока элемент не потеряет фокус.
* Выполните привязку к типу, допускающему значение NULL, например `int?` или `string`, и предоставьте пользовательскую логику для обработки недопустимых значений.
* Используйте [компонент для проверки формы](xref:blazor/forms-validation), например `InputNumber` или `InputDate`. Компоненты для проверки форм имеют встроенную поддержку управления недопустимыми входными данными. Компоненты для проверки форм:
  * позволяют пользователю вводить недопустимые данные и получать ошибки проверки в соответствующем контексте `EditContext`;
  * выводят ошибки проверки в пользовательском интерфейсе, не мешая пользователю вводить дополнительные данные в веб-форме.

## <a name="format-strings"></a>Строки формата

Привязка данных работает со строками формата <xref:System.DateTime> посредством атрибута [`@bind:format`](xref:mvc/views/razor#bind). Другие выражения форматирования, например денежные или числовые форматы, в настоящее время недоступны.

```razor
<input @bind="StartDate" @bind:format="yyyy-MM-dd" />

@code {
    [Parameter]
    public DateTime StartDate { get; set; } = new DateTime(2020, 1, 1);
}
```

В приведенном выше коде типом поля элемента `<input>` (`type`) по умолчанию является `text`. `@bind:format` поддерживается для привязки следующих типов .NET:

* <xref:System.DateTime?displayProperty=fullName>
* <xref:System.DateTime?displayProperty=fullName>?
* <xref:System.DateTimeOffset?displayProperty=fullName>
* <xref:System.DateTimeOffset?displayProperty=fullName>?

Атрибут `@bind:format` указывает формат даты, применяемый к значению `value` элемента `<input>`. Формат также используется для синтаксического анализа значения при возникновении события `onchange`.

Указывать формат для типа поля `date` не рекомендуется, так как в Blazor есть встроенная поддержка форматирования дат. Если все же для типа поля `yyyy-MM-dd` указывается формат, для правильной работы привязки используйте только формат даты `date`.

```razor
<input type="date" @bind="StartDate" @bind:format="yyyy-MM-dd">
```

## <a name="parent-to-child-binding-with-component-parameters"></a>Привязка родительского компонента к дочернему с помощью параметров компонентов

Привязка поддерживает параметры компонентов: с помощью атрибута `@bind-{PROPERTY}` можно выполнить привязку значения свойства от родительского компонента к дочернему. Привязка дочернего компонента к родительскому рассматривается в разделе [Привязка дочернего компонента к родительскому посредством цепочки привязки](#child-to-parent-binding-with-chained-bind).

Следующий дочерний компонент (`ChildComponent`) имеет параметр компонента `Year` и обратный вызов `YearChanged`:

```razor
<h2>Child Component</h2>

<p>Year: @Year</p>

@code {
    [Parameter]
    public int Year { get; set; }

    [Parameter]
    public EventCallback<int> YearChanged { get; set; }
}
```

Назначение структуры `EventCallback<T>` описывается в разделе <xref:blazor/event-handling#eventcallback>.

Рассмотрим представленный ниже родительский компонент.

* Он использует `ChildComponent`, а его параметр `ParentYear` привязывается к параметру `Year` дочернего компонента.
* Событие `onclick` используется для активации метода `ChangeTheYear`. Дополнительные сведения см. в разделе <xref:blazor/event-handling>.

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<p>ParentYear: @ParentYear</p>

<ChildComponent @bind-Year="ParentYear" />

<button class="btn btn-primary" @onclick="ChangeTheYear">
    Change Year to 1986
</button>

@code {
    [Parameter]
    public int ParentYear { get; set; } = 1978;

    private void ChangeTheYear()
    {
        ParentYear = 1986;
    }
}
```

При загрузке `ParentComponent` создается следующая разметка:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1978</p>

<h2>Child Component</h2>

<p>Year: 1978</p>
```

Если значение свойства `ParentYear` изменяется в результате нажатия кнопки в `ParentComponent`, свойство `Year` компонента `ChildComponent` обновляется. Новое значение свойства `Year` отображается в пользовательском интерфейсе при повторной отрисовке компонента `ParentComponent`:

```html
<h1>Parent Component</h1>

<p>ParentYear: 1986</p>

<h2>Child Component</h2>

<p>Year: 1986</p>
```

Параметр `Year` допускает привязку, так как он имеет сопутствующее событие `YearChanged`, соответствующее типу параметра `Year`.

В соответствии с соглашением `<ChildComponent @bind-Year="ParentYear" />` фактически эквивалентно следующему коду:

```razor
<ChildComponent @bind-Year="ParentYear" @bind-Year:event="YearChanged" />
```

Как правило, свойство можно привязать к соответствующему обработчику событий, включив атрибут `@bind-{PROPRETY}:event`. Например, свойство `MyProp` можно привязать к `MyEventHandler` с помощью следующих двух атрибутов:

```razor
<MyComponent @bind-MyProp="MyValue" @bind-MyProp:event="MyEventHandler" />
```

## <a name="child-to-parent-binding-with-chained-bind"></a>Привязка дочернего компонента к родительскому посредством цепочки привязки

Распространенным сценарием является связывание привязанного к данным параметра к элементу страницы в выходных данных компонента. Такой сценарий называется *цепочкой привязки*, так как одновременно имеется несколько уровней привязки.

Цепочку привязки нельзя реализовать с помощью синтаксиса `@bind` в элементе страницы. Обработчик событий и значение необходимо указывать отдельно. Однако родительский компонент может использовать синтаксис `@bind` с параметром компонента.

Приведенный ниже компонент `PasswordField` (*PasswordField.razor*) делает следующее:

* присваивает элементу `<input>` значение свойства `Password`;
* сообщает об изменениях свойства `Password` родительскому компоненту посредством [EventCallback](xref:blazor/event-handling#eventcallback);
* использует событие `onclick` для активации метода `ToggleShowPassword`. Дополнительные сведения см. в разделе <xref:blazor/event-handling>.

```razor
<h1>Child Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

@code {
    private bool _showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

Компонент `PasswordField` используется в другом компоненте:

```razor
@page "/ParentComponent"

<h1>Parent Component</h1>

<PasswordField @bind-Password="_password" />

@code {
    private string _password;
}
```

Для проверки пароля или перехвата ошибок в нем в предыдущем примере выполните указанные ниже действия.

* Создайте резервное поле для `Password` (`_password` в следующем примере кода).
* Выполните проверки или перехватите ошибки в методе задания `Password`.

В следующем примере пользователю немедленно сообщается о том, что в пароле есть пробел:

```razor
<h1>Child Component</h1>

Password: 

<input @oninput="OnPasswordChanged" 
       required 
       type="@(_showPassword ? "text" : "password")" 
       value="@Password" />

<button class="btn btn-primary" @onclick="ToggleShowPassword">
    Show password
</button>

<span class="text-danger">@_validationMessage</span>

@code {
    private bool _showPassword;
    private string _password;
    private string _validationMessage;

    [Parameter]
    public string Password
    {
        get { return _password ?? string.Empty; }
        set
        {
            if (_password != value)
            {
                if (value.Contains(' '))
                {
                    _validationMessage = "Spaces not allowed!";
                }
                else
                {
                    _password = value;
                    _validationMessage = string.Empty;
                }
            }
        }
    }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();

        return PasswordChanged.InvokeAsync(Password);
    }

    private void ToggleShowPassword()
    {
        _showPassword = !_showPassword;
    }
}
```

## <a name="radio-buttons"></a>Переключатели

Сведения о привязке к переключателям в форме см. в разделе <xref:blazor/forms-validation#work-with-radio-buttons>.
