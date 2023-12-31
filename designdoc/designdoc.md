# ML System Design Doc - [RU]
## Дизайн ML системы - Робот-Фасилитатор "Коллективный Разум" ака Майндмайндер 1.0.0

*Шаблон ML System Design Doc от телеграм-канала [Reliable ML](https://t.me/reliable_ml)*   

- Рекомендации по процессу заполнения документа (workflow) - [здесь](https://github.com/IrinaGoloshchapova/ml_system_design_doc_ru/blob/main/ML_System_Design_Doc_Workflow.md).  
- Детальный доклад о том, что такое ML System Design Doc и о том, как, когда и зачем его составлять - [тут](https://www.youtube.com/watch?v=PW9TGNr1Vqk).
    
> ## Термины и пояснения
> - Итерация - это все работы, которые совершаются до старта очередного пилота  
> - БТ - бизнес-требования 
> - EDA - Exploratory Data Analysis - исследовательский анализ данных  
> - `Product Owner`,  `Data Scientist` - роли, которые заполняют соответствующие разделы 
> - В этом шаблоне роль `Data Scientist` совмещает в себе компетенции классического `Data Scientist` с упором на исследования и `ML Engineer` & `ML Ops` роли с акцентом на продуктивизацию моделей
> - Для вашей организации распределение ролей может быть уточнено в зависимости от операционной модели 
> - ЦК - Цифровой Кооператив

### 1. Цели и предпосылки 
> ИДЕЯ! Любая коммуникация людей - это энергия. Как улучшить её качество так, чтобы все в команде были вдохновлены и не перегорали? Ответ - позволить ей течь без препятствий. 

> Для реализации идеи мы берём открытый исходный код приложения для конференц-связи и добавляем в него поддержку сценариев и плагинов с ИИ. 

> Плагины позволяют производить потоковую суммаризацию сути коммуникации, помогают строить задачники, и выполняют прочие функции ИИ, а сценарии - задают формат встречи. 

> Например, мы заметили, что формат совета, в котором слово передается каждому участнику конференции строго по кругу, и каждому на высказывание предоставляется ограниченное время по теме круга - позволяет сбалансировать потоки энергии участников, вдохновить их, и поддерживать в этом состоянии неограниченно долго. 

> В результате, компании и корпорации, которые применяют продукт сценарированной коммуникации в своих командах, повышают лояльность сотрудников к себе, а ИИ-плагины - создают экосистему полезных и приятных фич на основе голосовой информации.
#### 1.1. Зачем идем в разработку продукта?  

- Бизнес-цель: Создание системы эффективной и сбалансированной коммуникации для креативных производственных центров, интеллектуальных и экспертных сообществ. 
- Почему станет лучше, чем сейчас, от использования ML:
 Сейчас нету доступного к использованию инструмента для фасилитации и суммаризации групповых сессий. ML планируется для суммаризации. 
- Что будем считать успехом итерации с точки зрения бизнеса:  Будет готов свой сервер для групповых конференций, с поддержкой  суммаризации и поддержкой передачи слова средствами клиента, достаточной для записи видео-примера сессии, с которым можно будет пойти собирать требования и запросы в сообщества вне ЦК. Т.е. провести CustDev с хоть чем-то на руках.

#### 1.2. Бизнес-требования и ограничения  

- Краткое описание БТ и ссылки на детальные документы с бизнес-требованиями
>  ## Основные Функциональные Особенности
> - Фасилитация Групповых Сессий: Алгоритмическое управление передачей слова в кругу для баланса и эффективности общения.
> - Суммаризация: Автоматическая генерация кратких выводов от каждого участника, круга и всех кругов встречи.
>
> ## Специфические функциональные особенности
> - Построения Графа Связей: Визуализация сети отношений результатов встречи  
> - Построение Задачника: автоматическое формирование списка задач на основе информации из встречи с возможностью выгрузки во внешний таск-менеджер.

- Бизнес-ограничения `Product Owner`  
> * Для формата сессии передачи слова по кругу, рекомендуемый размер группы до семи человек
> * Размер сообщества ЦК сейчас около 100 активных участников, сумарную нагрузку на сервер конференц-связи в первой итерации можно ограничить этой цифрой.
> * Сервис должен различать командные слова и комментарии, и по разному их сегментировать

- Что мы ожидаем от конкретной итерации `Product Owner`. 
> * Возможность устраивать наглядную демонстрацию потенциальным пользователям функций управления сессией, транскрипции и сумаризации

- Описание бизнес-процесса пилота, насколько это возможно - как именно мы будем использовать модель в существующем бизнес-процессе? `Product Owner`    
> * Выше описан
- Что считаем успешным пилотом? Критерии успеха и возможные пути развития проекта `Product Owner`
> * Выше описано
> * Пути развития, прикрутить свою платежную/эмиссионную систему, и вознаграждать пользователей за вклад развития продукта(например правку транскрипции и суммаризации, отдачу прав на использование записанных материалов для улучшения продукта)


#### 1.3. Что входит в скоуп проекта/итерации, что не входит   

- На закрытие каких БТ подписываемся в данной итерации `Data Scientist`
> * Поднять self-hosted открытую платформу видео-конференции
> * Сегментации, транскрипции, суммаризации

- Что не будет закрыто `Data Scientist`  
> * Контроль качества моделей транскрипции и суммаризации.

- Описание результата с точки зрения качества кода и воспроизводимости решения `Data Scientist`  
> * На коленки написанный прототип, не предназначенный для масштабирования и детальной отладки, развития

- Описание планируемого технического долга (что оставляем для дальнейшей продуктивизации) `Data Scientist`  
> * В дальнейшим нужно прикрутить метрики для моделей транскрипции и сумаризации
> * Пайплайн сбора и сохранения данных из сессий(видео/аудио, полученные транскрпици и сумаризации, внесенные в них правки пользователями)
> * Оркестрацию с обратной нагрузочной связью для масштабирования динамического развертывания комнат с конференциями
> * Поддержку запуска разных версий инстанций сервера конференций в зависимости от обращающегося пользователя, с разным набором плагинов для поддержки A/B тестирования, и специальных клиентов 

#### 1.4. Предпосылки решения  

- Описание всех общих предпосылок решения, используемых в системе – с обоснованием от запроса бизнеса: какие блоки данных используем, горизонт прогноза, гранулярность модели, и др. `Data Scientist`
> * Набор предполагаемых  решений требует иследование на практике в первой волне внедрения.


### 2. Методология `Data Scientist`     

#### 2.1. Постановка задачи  

- Что делаем с технической точки зрения: рекомендательная система, поиск аномалий, прогноз, оптимизация, и др. `Data Scientist`  
> * Транскрипция аудио и суммаризация

#### 2.2. Блок-схема решения  
> TODO Оформление после подъема конференции, опробывания модели транскрипции на потоке аудио/видео

- Блок-схема для бейзлайна и основного MVP с ключевыми этапами решения задачи: подготовка данных, построение прогнозных моделей, оптимизация, тестирование, закрытие технического долга, подготовка пилота, другое. `Data Scientist`  
- [Пример возможной блок схемы](https://github.com/IrinaGoloshchapova/ml_system_design_doc_ru/blob/main/product_schema.png?raw=true)
> Схема обязательно включает в себя архитектуру бейзлайна. Если бейзлайн и основной MVP отличаются несущественно, то это может быть одна блок-схема. Если значительно, то рисуем две: отдельно для бейзлайна, отдельно для основного MVP.  
> Если блок-схема **шаблонна** - т.е. её можно скопировать и применить к разным продуктам, то она **некорректна**. Блок-схема должна показывать схему решения для конкретной задачи, поставленной в части 1.    

#### 2.3. Этапы решения задачи `Data Scientist`  
> Пока оставлю полежать. Всё требует исследования. TODO Расписать более внятно
>  Собственное решение для групповой конференции
> https://livekit.io
> Назначение, чтобы снять задачу сегментации сессии с ML и переложить пользователей конференции. Также, чтобы свободно получить доступ к отдельным потокам видео/аудио пользователей, и снять с генератора субтитров задачу выявления автора речи.
>
> Интеграция с существующими месенджерами
> * Telegram WebApp
> * Решения вокруг протокола Matrix
>
> Генерация субтитров для сумаризации
>* Whisper https://github.com/openai/whisper
>* Урок https://huggingface.co/learn/audio-course/chapter5/introduction
> 
> Генерация сумаризации
> Посмотреть в сторону LLM( ChatGPT, LLaMA, GigaChat  https://huggingface.co/ai-forever/ruGPT-3.5-13B )


- Для каждого этапа **по результатам EDA** описываем - **отдельно для бейзлайна** и **отдельно для основного MVP** - все про данные и технику решения максимально конкретно. Обозначаем необходимые вводные, технику предполагаемого решения и что ожидаем получить на выходе, чтобы перейти к следующему этапу.  
- Как правило, детальное и структурированное заполнение раздела `2.3` возможно только **по результатам EDA**.  
- Если описание в дизайн доке **шаблонно** - т.е. его можно скопировать и применить к разным продуктам, то оно **некорректно**. Дизайн док должен показывать схему решения для конкретной задачи, поставленной в части 1.  
    
> Примеры этапов:  
> - Этап 2 - Подготовка прогнозных моделей  
> - Этап 3 - Интерпретация моделей (согл. с заказчиком)  
> - Этап 4 - Интеграция бизнес правил для расчета бизнес-метрик качества мрдели  
> - Этап 5 - Подготовка инференса модели по итерациям    
> - Этап 6 - Интеграция бизнес правил  
> - Этап 7 - Разработка оптимизатора (выбор оптимальной итерации)  
> - Этап 8 - Подготовка финального отчета для бизнеса  

*Этап 1 - это обычно, подготовка данных.*  

В этом этапе должно быть следующее:  

- Данные и сущности, на которых будет обучаться ваша модель машинного обучения. Отдельная таблица для целевой переменной (либо целевых переменных разных этапов), отдельная таблица – для признаков.  
  
| Название данных  | Есть ли данные в компании (если да, название источника/витрин) | Требуемый ресурс для получения данных (какие роли нужны) | Проверено ли качество данных (да, нет) |
| ------------- | ------------- | ------------- | ------------- |
| Продажи | DATAMARTS_SALES_PER_DAY  | DE/DS | + |
| ...  | ...  | ... | ... |
 
- Краткое описание результата этапа - что должно быть на выходе: витрины данных, потоки данных, др.  
  
> Чаще всего заполнение раздела невозможно без EDA.

 *Этапы 2 и далее, помимо подготовки данных.*
 
Описание техники **для каждого этапа** должно включать описание **отдельно для MVP** и **отдельно для бейзлайна**:  

- Описание формирования выборки для обучения, тестирования и валидации. Выбор репрезентативных данных для экспериментов, обучения и подготовки пилота (от бизнес-цели и репрезентативности данных с технической точки зрения) `Data Scientist`    
- Горизонт, гранулярность, частоту необходимого пересчета прогнозных моделей `Data Scientist`   
- Определение целевой переменной, согласованное с бизнесом `Data Scientist`   
- Какие метрики качества используем и почему они связаны с бизнес-результатом, обозначенным `Product Owner` в разделах `1` и `3`. Пример - WAPE <= 50% для > 80% категорий, bias ~ 0. Возможна формулировка в терминах относительно бейзлайна, количественно. Для бейзлайна могут быть свои целевые метрики, а может их вообще не быть (если это обосновано) `Data Scientist`   
- Необходимый результат этапа. Например, необходимым результатом может быть не просто достижение каких-либо метрик качества, а включение в модели определенных факторов (флаг промо для прогноза выручки, др.) `Data Scientist`    
- Какие могут быть риски и что планируем с этим делать. Например, необходимый для модели фактор (флаг промо) окажется незначимым для большинства моделей. Или для 50% моделей будет недостаточно данных для оценки `Data Scientist`    
- Верхнеуровневые принципы и обоснования для: feature engineering, подбора алгоритма решения, техники кросс-валидации, интерпретации результата (если применимо).  
- Предусмотрена ли бизнес-проверка результата этапа и как будет проводиться `Data Scientist` & `Product Owner`  
  
### 3. Подготовка пилота  
  
#### 3.1. Способ оценки пилота  
  
- Краткое описание предполагаемого дизайна и способа оценки пилота `Product Owner`, `Data Scientist` with `AB Group` 
  
#### 3.2. Что считаем успешным пилотом  
  
Формализованные в пилоте метрики оценки успешности `Product Owner`   
  
#### 3.3. Подготовка пилота  
  
- Что можем позволить себе, исходя из ожидаемых затрат на вычисления. Если исходно просчитать сложно, то описываем этап расчетов ожидаемой вычислительной сложности на эксперименте с бейзлайном. И предусматриваем уточнение параметров пилота и установку ограничений по вычислительной сложности моделей. `Data Scientist` 

### 4. Внедрение `для production систем, если требуется`    

> Заполнение раздела 4 требуется не для всех дизайн документов. В некоторых случаях результатом итерации может быть расчет каких-то значений, далее используемых в бизнес-процессе для пилота.  
  
#### 4.1. Архитектура решения   
  
- Блок схема и пояснения: сервисы, назначения, методы API `Data Scientist`  
  
#### 4.2. Описание инфраструктуры и масштабируемости 
  
- Какая инфраструктура выбрана и почему `Data Scientist` 
- Плюсы и минусы выбора `Data Scientist` 
- Почему финальный выбор лучше других альтернатив `Data Scientist` 
  
#### 4.3. Требования к работе системы  
  
- SLA, пропускная способность и задержка `Data Scientist`  
  
#### 4.4. Безопасность системы  
  
- Потенциальная уязвимость системы `Data Scientist`  
  
#### 4.5. Безопасность данных   
  
- Нет ли нарушений GDPR и других законов `Data Scientist`  
  
#### 4.6. Издержки  
  
- Расчетные издержки на работу системы в месяц `Data Scientist`  
  
#### 4.5. Integration points  
  
- Описание взаимодействия между сервисами (методы API и др.) `Data Scientist`  
  
#### 4.6. Риски  
  
- Описание рисков и неопределенностей, которые стоит предусмотреть `Data Scientist`   
  
> ### Материалы для дополнительного погружения в тему  
> - [Шаблон ML System Design Doc [EN] от AWS](https://github.com/eugeneyan/ml-design-docs) и [статья](https://eugeneyan.com/writing/ml-design-docs/) с объяснением каждого раздела  
> - [Верхнеуровневый шаблон ML System Design Doc от Google](https://towardsdatascience.com/the-undeniable-importance-of-design-docs-to-data-scientists-421132561f3c) и [описание общих принципов его заполнения](https://towardsdatascience.com/understanding-design-docs-principles-for-achieving-data-scientists-53e6d5ad6f7e).
> - [ML Design Template](https://www.mle-interviews.com/ml-design-template) от ML Engineering Interviews  
> - Статья [Design Documents for ML Models](https://medium.com/people-ai-engineering/design-documents-for-ml-models-bbcd30402ff7) на Medium. Верхнеуровневые рекомендации по содержанию дизайн-документа и объяснение, зачем он вообще нужен  
> - [Краткий Canvas для ML-проекта от Made with ML](https://madewithml.com/courses/mlops/design/#timeline). Подходит для верхнеуровневого описания идеи, чтобы понять, имеет ли смысл идти дальше.  
