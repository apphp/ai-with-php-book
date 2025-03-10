# Архитектура агента ИИ

При проектировании агентов искусственного интеллекта архитектура играет ключевую роль в определении того, как агент воспринимает, принимает решения и действует в своей среде. Существует три основных типа архитектур ИИ-агентов: **реактивная**, **целенаправленная** (делиберативная) и **гибридная**. Каждая из них обладает уникальными характеристиками, которые подходят для разных приложений — от систем с мгновенной реакцией до стратегического принятия решений.

<div align="left"><figure><img src="../../.gitbook/assets/ai-agent-architecture-min.png" alt="" width="375"><figcaption><p>Архитектура агента ИИ</p></figcaption></figure></div>

### 1. Реактивная архитектура

Реактивные архитектуры — это самая простая форма проектирования агентов ИИ. Такие агенты реагируют напрямую на стимулы окружающей среды, не имея внутренней памяти или процесса рассуждения. Они опираются на заранее запрограммированные правила или соответствия, которые определяют их реакции исключительно на основе входных данных.

Эта архитектура особенно эффективна в быстро меняющихся или непредсказуемых средах, где критически важны мгновенные реакции. Примеры:

* **Объезд препятствий в беспилотных автомобилях**: В автономных транспортных средствах реактивные системы обнаруживают препятствия, такие как пешеходы или другие машины. Такие реакции, основанные на данных датчиков, позволяют машине мгновенно реагировать без сложного планирования.
* **Роботы на складах**: Роботы, используемые, например, на складах Amazon, применяют реактивные правила для передвижения между стеллажами, избегания препятствий и выполнения задач по подбору товаров. Им достаточно базовых данных от сенсоров и простых правил принятия решений для эффективной работы в реальном времени.
* **Боты для торговли акциями**: В финансовых рынках реактивные боты анализируют изменения цен в режиме реального времени и моментально совершают сделки на основе заданных критериев, таких как покупка акций при снижении их цены ниже определенного порога.

Хотя реактивные агенты быстры и эффективны в выполнении простых задач, им не хватает способности адаптироваться, обучаться или планировать действия на будущее. В результате их реакции могут быть ограничены в случае сложных или новых сценариев.

### 2. Целенаправленная (делиберативная) архитектура

Делиберативные архитектуры, также известные как целенаправленные или основанные на модели системы, позволяют агентам рассуждать о своих действиях с использованием внутренней модели окружающей среды. Эта модель позволяет агентам принимать решения, основываясь на целях, планах и ожидаемых результатах, что обеспечивает более стратегическое и обоснованное поведение.

Такие агенты особенно полезны в приложениях, требующих тщательного планирования и дальновидности, где реакции должны соответствовать долгосрочным целям. Примеры:

* **Автономные марсоходы**: Марсоходы NASA, такие как **Curiosity**, используют делиберативные архитектуры для планирования маршрутов и принятия стратегических решений по исследованию. Оценивая рельеф и энергетические ограничения, они устанавливают и реализуют цели, например, движение к геологической цели.
* **Системы медицинской диагностики**: Делиберативные агенты в здравоохранении используют данные пациентов, симптомы и базы медицинских знаний для оценки возможных диагнозов, рекомендаций по лечению и прогнозирования исходов. Этот подход, основанный на планировании, критически важен для сложного и многогранного процесса принятия решений.
* **ИИ для игры в шахматы**: Игры, такие как шахматы, требуют от агентов умения предвидеть и оценивать возможные ходы. Делиберативные архитектуры позволяют шахматным ИИ моделировать множество сценариев, принимать обдуманные решения и разрабатывать долгосрочные стратегии, как это было с системой **Deep Blue** в исторических матчах.

Несмотря на высокую способность делиберативных агентов к сложному принятию решений, они часто требуют больших вычислительных ресурсов. Необходимость обработки в реальном времени может стать ограничением в ситуациях, где важна оперативность.

### 3. Гибридная архитектура

Гибридные архитектуры объединяют элементы как реактивных, так и делиберативных систем, используя сильные стороны каждой из них для создания агентов, которые могут быстро реагировать на стимулы и одновременно иметь возможность стратегического планирования. В такой архитектуре агент может использовать реактивный слой для мгновенных реакций и делиберативный слой для постановки высокоуровневых целей и планирования.

Этот подход идеален для сред, где требуются как немедленные действия, так и долгосрочные стратегии. Примеры:

* **Гуманоидные роботы**: В таких задачах, как спасательные миссии, гибридные роботы должны быстро реагировать на препятствия и одновременно планировать маршруты для достижения цели — например, к застрявшим людям. Реактивный слой обрабатывает внезапные препятствия, а делиберативный слой занимается навигацией к месту спасения.
* **Интеллектуальные виртуальные ассистенты**: Виртуальные помощники, такие как **Siri** или **Google Assistant**, построены на гибридных архитектурах. Для простых команд, таких как установка таймера, они реагируют мгновенно. Для более сложных задач, например, планирования дня с учетом предпочтений и встреч пользователя, они используют делиберативное планирование, учитывающее множество факторов.
* **Автономные дроны для наблюдения**: Дроны с гибридной архитектурой могут избегать непосредственных угроз, таких как птицы или линии электропередач, и одновременно выполнять установленный маршрут патрулирования. Реактивная система управляет корректировкой полета в реальном времени, а делиберативная система обеспечивает выполнение заданного маршрута.

Гибридные агенты являются универсальными и адаптивными, сочетая скорость реактивных ответов с глубиной делиберативного планирования. Такой двухуровневый подход делает их подходящими для сложных реальных сред, где необходимы как гибкость, так и дальновидность.
