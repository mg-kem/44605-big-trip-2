# Большое путешествие — Trip Planner Application

## Live Demo


## GitHub
https://github.com/htmlacademy-ecmascript/44605-big-trip-2

---

## О проекте

«Большое путешествие» — клиентское веб-приложение для планирования маршрута поездки. Пользователь может добавлять точки маршрута (события), выбирать тип (перелёт, поезд, корабль и др.), даты, направления и опции, редактировать и удалять точки. Реализованы фильтрация по типу поездки, сортировка (по дате, времени, цене), расчёт стоимости поездки. Данные загружаются и сохраняются через REST API.

---

## Стек технологий

| Категория | Технология |
|-----------|-------------|
| **Язык** | JavaScript (ES6+) |
| **Сборка** | Webpack 5 |
| **Даты** | dayjs, dayjs-plugin-utc |
| **Выбор даты/времени** | flatpickr |
| **Архитектура** | MVP (Model-View-Presenter) |
| **HTTP** | Fetch API (через ApiService) |

---

## Features

- загрузка точек маршрута с REST API
- добавление, редактирование и удаление точек
- фильтрация по типу поездки
- сортировка по дате, времени, цене
- расчёт общей стоимости поездки
- блокировка UI во время запросов к API
- кастомный фреймворк представлений (AbstractView, Observable)

---

## Architecture

Проект построен по принципу MVP:

- **Model** — данные (PointsModel, FilterModel) и работа с API (PointApiService)
- **View** — отображение (view-компоненты: PointView, FilterView, SortView и др.)
- **Presenter** — связь Model и View, обработка действий пользователя (BoardPresenter, PointPresenter, FilterPresenter и др.)

Используются:

- абстрактные классы представлений (`src/framework/view/`)
- наблюдатель (Observable) для обновления состояния
- единый ApiService с поддержкой авторизации

---

## Структура приложения и поток данных

### Точка входа

**`src/main.js`** — инициализация приложения:

- Создаёт экземпляры `PointsModel` (с внедрённым `PointApiService`) и `FilterModel`.
- Создаёт `BoardPresenter` с контейнерами для шапки (`.trip-main`) и списка точек (`.trip-events`).
- Вызывает `boardPresenter.init()` и `pointsModel.init()` — загрузка точек с API и отрисовка интерфейса.

### Presenters и View

- **BoardPresenter** — координирует HeaderPresenter, FilterPresenter, SortView, список точек (PointPresenter / NewPointPresenter), отображает итоговую информацию о поездке и кнопку добавления точки.
- **PointPresenter** — одна точка маршрута: отображает PointView, при клике на редактирование — PointEditView, обрабатывает обновление/удаление через модель.
- **NewPointPresenter** — форма создания новой точки (NewPointView).
- **FilterPresenter** — фильтры по типу поездки, синхронизация с FilterModel.
- **HeaderPresenter** — информация о маршруте (TripInformationView), кнопка «New event».

### Модели и сервисы

- **PointsModel** — список точек маршрута, методы добавления/обновления/удаления, подписка на изменения (Observable). При инициализации запрашивает точки с API.
- **FilterModel** — текущий выбранный фильтр.
- **PointApiService** (наследует **ApiService**) — запросы к API: точки маршрута, направления, предложения; авторизация через заголовок.

### Константы и утилиты

- **`src/const.js`** — API_URL, AUTHORIZATION, форматы дат (dayjs), типы сортировки, действия пользователя (UserAction), типы обновления (UpdateType), статусы формы и др.
- **`src/utils.js`** — вспомогательные функции (в т.ч. для дат и сортировки).
- **`src/framework/`** — AbstractView, AbstractStatefulView, Observable, render, ApiService, UiBlocker.

### Сборка

- **Webpack** — конфигурация в `webpack.config.js`, dev-сервер с hot reload, сборка для production.

---

## Запуск и сборка

```bash
npm install   # установка зависимостей
npm start     # dev-сервер (Webpack)
npm run build # сборка для production
npm run lint  # проверка линтером (ESLint)
```

---

<a href="https://htmlacademy.ru/intensive/ecmascript"><img align="left" width="50" height="50" title="HTML Academy" src="https://up.htmlacademy.ru/static/img/intensive/ecmascript/logo-for-github.svg"></a>

Репозиторий создан для обучения на профессиональном курсе «[JavaScript. Архитектура клиентских приложений](https://htmlacademy.ru/intensive/ecmascript)» от [HTML Academy](https://htmlacademy.ru).
