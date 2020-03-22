# Задание №8

В этом задании вам нужно написать `case-study` о том как вы применили знания, полученные на курсе, к своим проектам.

## To start
## Описание проекта
- Проект обслуживающий страховой бизнес а именно помогающий реализовать экономию расходов на страховании автомобиля. Основная идея - чем лучший вы водитель тем меньше тратите на страховании. Заказчик страховая компания. Состоит из нескольких основных частей: Монолит содержаший: АПИ мобильных устройств (Grape), АПИ для коммуникации с внешними сервисами, АПИ для коммуникации с сервисом телематики, АПИ для бизнеса(Grape), аутентификация/авторизация, бизнес-дата, аналитика. Фронт небольшой используется React. Вторая часть: сервис импорта бигдаты с телематики и формировании отчетности из нее. 
- Проекту около двух лет с февраля 2019го в проде. Пришел на проект в марте 2019 задачей было сменить предыдущую команду разрабочиков, сопровождать и развивать проект. Ситуация практически идеальна - green field и никакого legacy.
- Проект неполохо написан и придуман, но с развитием появилось несколько проблем с перфомансом, особенно достает то, что -  проект не является особо высоконагруженным - 300-500RPM. несколько сот гб БД, адекватная мощность инфраструктуры. Актуализированны следующие проблеммы:
    - Размер и производительность БД
    - Производительность АПИ
    - Производительность инфраструктуры

## Шаг первый Размер и производительность БД (реализован)
- С помощью sentry были определены дедлоки, запросы переписаны с использованием materialized viws
- В skylight были обнаружены проблемы с производительностью некоторых эндпойнтов отдающих данные с больших таблиц
	- Обновление ruby до версии 2,6,3, rails 5.2 (по рекомендациям курса) было отмечено снижение времени проблемных ответов по некоторым эндпойнтам на 70-90%	
	- Добавление pghero в прод, по рекомендациям pghero - добавление индексов (по материалам курса) - снижение проблемных ответов в эндпойнтах берушщих данные по большым табицам на 20-70% 
* Примечание - дальнейшеие работы в этом направлении пока отложены, точками роста определены: Sidekiq, Инфраструктура.

## Шаг второй Sidekiq (запланирован)
- Обнаружено постоянное разбухание сайдкик воркеров на импорте большеразмерных данных в формате json
* Запланировано
    - Ап версии sidekiq, redis (по рекомендациям курса)
    - Добавление OJ, рефакторинг обработок json с использованием oj и потокового чтения больших данных (по материалам курса)

## Шаг третий Инфраструктура (запланирован, частично реализуется)
-Обнаружена проблема с инфраструктурой сервера прод. Это dedicated, довольно мощный - несколько десятков ядер, несколько десятков гб оперативной памяти несколько сотен гб дискового пространства, при этом частые проблемы с переполнением оперативной памяти и жесткого дика, невысокая нагрузка на процессоры (избыточность ресурсов процессора) при общей цене на обслуживание сравнимой с ценой развертывания и сопровождени AWS кластера отвечающего задачам приложения.
* Запланировано
    - Оптимизация окружения разрабоки, оптимизация тестов (по материалам курса), добавлен CI, оптимизация CI (по материалам курса) - Реализовано
    - Подготовка к реализация переноса на AWS ECS - подготовленны и откомпозирована сборка для построени кластера - Реализовано
    - Перенос инфраструктуры стейджа
    - Реализация разширенного мониторинга собственными средствами - с использованием prometeus, graphana (по материалам курса)
    - CD
    - Перенос инфраструктуры прода

## Summary
Пока говорить о каких-то результатах рано. Узнал о нескольких интересных инструментах, узнал о системных подходах, определил предмет и метод в подходах к оптимизации. Что-то пробую реализовать. Большая часть информации требует повторного просмотра и осмысления. Какая-то часть курса пока не применима к реализации на текущих проектах,  например - фронта тут практически нет. Но пригодиться в будущем.