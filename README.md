﻿# Dependency Injection Container

## Самостоятельная подготовка

Посмотрите видеолекцию [DI-контейнеры](https://ulearn.me/Course/cs2/Vviedieniie_93d19beb-1465-430f-ac12-03f40ebd3e17) (~1.5 часов)

## Практика работы с DI-контейнером

1. Разминка. В классе Program переделайте Main так, чтобы MainForm 
создавался контейнером. Удалите у MainForm конструктор без параметров 
и сделайте так, чтобы контейнер инжектировал в MainForm список IUiAction.

2. INeed<T>. Изучите код KochFractalAction. 
Изучите механику работы INeed<T> и DependencyInjector.
Оцените такой подход к управлению зависимостями.


3. Рефакторинг. Измените класс KochFractalAction так, 
чтобы его зависимости IImageHolder и Pallette инжектировались 
явно через конструктор, без использования интерфейса INeed.

  Подсказка 1. Окружите создание MainForm в try-catch, 
  и выводите текст исключения на экран. Это поможет с отладкой.

  `MessageBox.Show(ex.ToString())`

  Подсказка 2. Сложность в том, чтобы в MainForm и KochFractalAction 
  оказались ссылки на один и тот же объект PictureBoxImageHandler.

4. Еще рефакторинг. Изучите KochFractalAction и поймите, что 
на самом деле IImageHolder и Pallette ему не нужны. Измените его так,
чтобы он принимал только KochPainter. 

5. Фабрика. Аналогично удалите INeed из класса DragonFractalAction.
Дополнительное ограничение — нельзя менять публичный интерфейс DragonPainter.
Особенность в том, что одна из зависимостей DragonPainter — 
DragonSettings оказывается известной только в процессе работы экшена.
Из-за этого вы не можете просить инжектировать в конструктор уже готовый Painter.
Вместо этого, используйте возможность инжектировать фабрику.
https://github.com/ninject/Ninject.Extensions.Factory/wiki/Factory-interface

	Подсказка. Ninject.Extensions.Factory нужно установить через NuGet.

6. Новая зависимость. Переведите DragonPainter на использование цветов палитры, 
как это сделано в KochPainter. 

Убедитесь, что экшен настройки палитры работает как надо.

7. Источник зависимости. Аналогично отрефакторите ImageSettingsAction.
Попробуйте придумать, как сделать так, чтобы ImageSettingsAction принимал 
в качестве зависимости не IImageSettingsProvider, а сам ImageSettings.

8. Избавьтесь от остальных использований INeed и удалите этот интерфейс 
и класс DependencyInjector из проекта.

9. Обратите внимание на многочисленные привязки к IUiAction. В реальных
проектах количество классов может исчисляться десятками и сотнями. Воспользуйтесь
документацией https://github.com/ninject/Ninject.Extensions.Conventions 
и найдите, как все эти привязки сделать в одну строчку. 