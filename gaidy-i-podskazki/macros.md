# \[dev\] Macros

{% hint style="warning" %}
Скрипты устарели и требуют доработки
{% endhint %}

Все скрипты нужно сохранять в папке `macros`, которая находится в папке с игрой по пути`.iPlayCraft/updates/<сервер>/liteconfig/common/`

Если в редакторе в Minecraft кириллица отображается вопросиками, то необходимо сохранить файл в кодировке `Windows-1251`

## Отделение ЛС от общего чата 

[https://youtu.be/WmBvSoZj3DQ](https://youtu.be/WmBvSoZj3DQ)

## Мат-фильтр



[https://www.youtube.com/watch?v=0lHXSfP8Pb0](https://www.youtube.com/watch?v=0lHXSfP8Pb0)

## Отображение режимов

Скрипт позволяет вывести на экран текущее состояние `god`, `vanish`, и тому подобные.

{% tabs %}
{% tab title="Установка" %}
Видео: [https://youtu.be/o17C4xeuN38](//youtu.be/o17C4xeuN38)
{% endtab %}

{% tab title="Исходный код God v2" %}
```c
// OnChat

$${

IFMATCHES(%CHATCLEAN%, "\[?\] Режим бессмертия успешно включен!);
	SETLABEL(g1, "&3GOD &8- &aON");
	SETLABEL(g2, "&3GOD &8- &aON");
ENDIF;

IFMATCHES(%CHATCLEAN%, "\[?\] Режим бессмертия успешно выключен!);
	SETLABEL(g1, "&4GOD &8- &cOFF");
	SETLABEL(g2, "&4GOD &8- &cOFF");
ENDIF;

}$$
```
{% endtab %}

{% tab title="Исходный код Vanish" %}
```c
// OnChat

$${

IFMATCHES(%CHATCLEAN%, "You have vanished. Poof.");
	SETLABEL(v1, "&3VANISH &8- &aON");
	SETLABEL(v2, "&3VANISH &8- &aON");
ENDIF;

IFMATCHES(%CHATCLEAN%, "You have joined vanished. To appear: /vanish");
	SETLABEL(v1, "&3VANISH &8- &aON");
	SETLABEL(v2, "&3VANISH &8- &aON");
ENDIF;

IFMATCHES(%CHATCLEAN%, "You have become visible.");
	SETLABEL(v1, "&4VANISH &8- &cOFF");
	SETLABEL(v2, "&4VANISH &8- &cOFF");
ENDIF;

}$$


// OnJoinGame

$${

SETLABEL(v1, "&4VANISH &8- &cOFF");
SETLABEL(v2, "&4VANISH &8- &cOFF");

}$$
```
{% endtab %}
{% endtabs %}

## Логирование сообщений в файлы

Скрипт позволяет разделить сообщения по типам и сохранять их в соответствующие файлы:

* `[П][ЛС]` – Подслушанные личные сообщения
* `[П]` – Подслушанный локальный чат
* `[М]` – Мировой глобальный чат
* `[О]` – Все сообщения \(общий файл\)

{% tabs %}
{% tab title="Установка" %}
Поместить файл `tree.txt` в папку `macros`  
В игре в событие `OnChat` написать `$$<tree.txt>`

{% file src="../.gitbook/assets/tee.txt" caption="Скачать tree.txt" %}

Файлы с логами будут в `.iPlayCraft/updates/<сервер>/liteconfig/common/macros/logs/`
{% endtab %}

{% tab title="Исходный код" %}
```c
// OnChat

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



