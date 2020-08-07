# Macros

{% hint style="warning" %}
Если в редакторе в Minecraft кириллица отображается вопросиками, то необходимо сохранить файл в кодировке `Windows-1251`
{% endhint %}

## Разделение 



## Логирование сообщений в файлы

Скрипт позволяет разделить сообщения по типам и сохранять их в соответствующие файлы:

* `[П][ЛС]` – Подслушанные личные сообщения
* `[П]` – Подслушанный локальный чат
* `[М]` – Мировой глобальный чат
* `[О]` – Все сообщения \(общий файл\)

{% tabs %}
{% tab title="Установка" %}
Поместить файл `tree.txt` в `.iPlayCraft/updates/<сервер>/liteconfig/common/macros`  
В игре в событие `OnChat` написать `$$<tree.txt>`

{% file src="../.gitbook/assets/tee.txt" caption="Скачать tree.txt" %}

Файлы с логами будут в `.iPlayCraft/updates/<сервер>/liteconfig/common/macros/logs`
{% endtab %}

{% tab title="Исходный код" %}
```c
$${

// Подслушанный ЛС
IFMATCHES("%CHATCLEAN%", "^\[П\]\[ЛС\]\[(.+?) -> (.+?)] (.+?)$");
    match(%DATE%, "(.+?)-(.+?)-(.+?)$", {#y,#m,#d})
    if(#d<10); &d="0%#d%"; else; &d="%#d%"; endif;
    if(#m<10); &m="0%#m%"; else; &m="%#m%"; endif;
    logto([П][ЛС]%&d%.%&m%.%#y%.txt, "[%TIME%] %CHATCLEAN%")
ENDIF;

// Подслушанный локал
IFMATCHES("%CHATCLEAN%", "^\[П\]\[.+\]\[(.+?)\] » (.+?)$");
    match(%DATE%, "(.+?)-(.+?)-(.+?)$", {#y,#m,#d})
    if(#d<10); &d="0%#d%"; else; &d="%#d%"; endif;
    if(#m<10); &m="0%#m%"; else; &m="%#m%"; endif;
    logto([П]%&d%.%&m%.%#y%.txt, "[%TIME%] %CHATCLEAN%")
ENDIF;

// Мировой чат
IFMATCHES("%CHATCLEAN%", "^\[М\]\[.+\]\[(.+?)\] » (.+?)$");
    match(%DATE%, "(.+?)-(.+?)-(.+?)$", {#y,#m,#d})
    if(#d<10); &d="0%#d%"; else; &d="%#d%"; endif;
    if(#m<10); &m="0%#m%"; else; &m="%#m%"; endif;
    logto([M]%&d%.%&m%.%#y%.txt, "[%TIME%] %CHATCLEAN%")
ENDIF;

// Общий лог
match(%DATE%, "(.+?)-(.+?)-(.+?)$", {#y,#m,#d})
if(#d<10); &d="0%#d%"; else; &d="%#d%"; endif;
if(#m<10); &m="0%#m%"; else; &m="%#m%"; endif;
logto([O]%&d%.%&m%.%#y%.txt, "[%TIME%] %CHATCLEAN%")

}$$
```
{% endtab %}
{% endtabs %}



