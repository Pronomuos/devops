## **Docker: Мультистейджинг, различные уровни зависимостей**

Цель лабораторной: освоить методы создания образов с разными уровнями зависимостей

1. Для системных зависимостей приложения создаем образ name:system
2. Для зависимостей сборки создаем образ name:build
3. Для приложения создаем образ name:app
4. Вся конфигурация выполняется через переменные окружения
5. Освоить параметр --cache-from
6. Образ должен быть на базе scratch

Выполена в рамках второй лабы.