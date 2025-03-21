# Среда агента ИИ

Искусственные интеллектуальные агенты функционируют в различных средах, которые определяют, как они воспринимают, взаимодействуют и добиваются успеха в выполнении задач. Понимание этих сред имеет ключевое значение для проектирования эффективных систем ИИ. Согласно **Расселу** и **Норвигу**, среды могут быть охарактеризованы определёнными особенностями, каждая из которых влияет на восприятие и действия агентов. В этом тексте мы рассмотрим эти особенности с примерами и объясним, как агенты адаптируют своё восприятие и действия к каждому типу среды.

<div align="left"><figure><img src="../../.gitbook/assets/ai-agent-environment-min.png" alt="" width="375"><figcaption><p>Среда агента ИИ</p></figcaption></figure></div>

### Особенности среды

При проектировании интеллектуальных агентов понимание среды играет ключевую роль в определении того, как они будут воспринимать, действовать и обучаться. Согласно структуре, предложенной Расселом и Норвигом в книге _“Искусственный интеллект: современный подход”_, среды отличаются по ряду характеристик. Эти особенности влияют на поведение агента и определяют методы и алгоритмы, используемые для достижения целей. Давайте подробнее рассмотрим каждую из характеристик.

### 1. Полностью наблюдаемая vs Частично наблюдаемая

Наблюдаемая среда определяет, сколько информации агент может получить о своем текущем состоянии.

Среда является полностью наблюдаемой, если агент в любой момент времени имеет доступ ко всей релевантной информации о текущем состоянии. В таких условиях агенты работают без неопределенности, что упрощает разработку четких стратегий. Например, в настольной игре, такой как шахматы, агент может наблюдать всю доску, зная расположение всех фигур.

* **Пример**: В шахматной игре ИИ видит всю доску, что позволяет ему прогнозировать все возможные ходы соперника. Эта прозрачность способствует стратегическому планированию, так как агенту не нужно гадать или делать выводы о недостающей информации.
* **Восприятие и действия агента**: Здесь агент может использовать прямые наблюдения для построения полной модели мира, исключая необходимость в вероятностных рассуждениях. Агент действует на основе этого полностью информированного представления, что позволяет разрабатывать сложные и детерминированные стратегии.

Напротив, в частично наблюдаемых средах доступна лишь ограниченная или зашумленная информация, что создает неопределенность относительно состояния. Агенты в таких условиях часто вынуждены делать прогнозы или опираться на предыдущие наблюдения, чтобы восполнить недостающие данные. Примером может служить автономный автомобиль, который воспринимает свое окружение через датчики, которые могут быть заблокированы препятствиями или ограничены плохим освещением, требуя использования предсказательных алгоритмов для безопасного передвижения.

* **Пример**: Автономный автомобиль работает в условиях частичной видимости, полагаясь на данные с датчиков, которые могут быть ограничены слепыми зонами или низкой освещенностью. Он должен прогнозировать и предугадывать потенциальные опасности для обеспечения безопасного движения.
* **Восприятие и действия агента**: Агент должен полагаться на предыдущие наблюдения, вероятностные модели или внутреннюю память, чтобы восполнить пробелы в информации. Его действия адаптируются в зависимости от оценочного состояния среды, что часто включает в себя осторожные или консервативные выборы для управления неопределенностями.

### 2. Статическая vs Динамическая среда

Динамический аспект среды определяется тем, насколько она меняется со временем, независимо от вмешательства агента.

Статическая среда остается неизменной в течение выполнения операций агентом, позволяя ему планировать действия без учета изменений в среде. Такие среды проще, так как агенты могут принимать решения без необходимости обновлять информацию. Классическим примером статической среды являются головоломки, где среда не меняется после начала задачи.

* **Статическая среда**: В статической среде мир остается неизменным, пока агент принимает решения или выполняет действия, что позволяет более точное и просчитанное планирование.
* **Пример**: В кроссворде агент анализирует статичную доску, чтобы определить, какие слова подходят в заданные клетки, без внешних вмешательств.

Динамическая среда, напротив, меняется независимо от действий агента, требуя от него постоянной адаптации. Динамические среды предъявляют высокие требования к скорости реакции агентов и часто заставляют их принимать решения на основе неполных данных. Например, алгоритм для торговли на бирже работает в динамической среде, где рыночные цены постоянно колеблются, а решения требуют быстроты и точности.

* **Пример**: Алгоритм биржевой торговли ИИ должен постоянно отслеживать колебания рыночных цен и адаптировать свои стратегии в реальном времени, чтобы максимизировать прибыль или минимизировать риски.
* **Восприятие и действия агента**: В таких средах агент должен непрерывно воспринимать изменения и немедленно на них реагировать. Его действия могут включать адаптивные стратегии или использование вероятностного моделирования для реакции на непредсказуемые или быстрые изменения.

### 3. Дискретная vs Непрерывная среда

Дискретность или непрерывность среды относится к природе возможных состояний и действий.

Дискретная среда включает конечный набор четко определённых состояний и действий. Это упрощает процесс принятия решений, поскольку агент может заранее спланировать свои действия. Например, такие игры, как “крестики-нолики”, являются дискретными, так как у агента ограниченное количество ходов и конфигураций игрового поля для анализа.

* **Пример**: В игре “крестики-нолики” агент ИИ анализирует каждую клетку и ограниченный набор возможных ходов для точного принятия решений.
* **Восприятие и действия агента**: Агент воспринимает дискретные состояния и связывает действия с этими конкретными возможностями, что приводит к структурированному процессу принятия решений. Он рассчитывает оптимальные ходы на основе предсказуемых результатов и ограниченного пространства состояний.

Непрерывная среда, напротив, включает бесконечное множество возможных состояний и действий. Агенты в таких средах, например, роботизированная рука, которая настраивает своё положение, требуют более сложных алгоритмов для выполнения постепенных корректировок. Непрерывные среды часто требуют точных вычислений и управления в реальном времени для выполнения плавных действий.

* **Пример**: Дрон, передвигающийся в воздушном пространстве, постоянно корректирует свой маршрут, учитывая такие факторы, как высота, скорость и условия ветра.
* **Восприятие и действия агента**: Агент выполняет непрерывные, тонкие корректировки в своём восприятии и действиях. Вместо дискретного отображения состояний он полагается на математические модели и данные сенсоров в реальном времени для поддержания контроля и эффективной навигации.

### 4. Детерминированная vs Стохастическая среда

Предсказуемость среды определяет, насколько определёнными или неопределёнными являются действия агента относительно их результатов.

Детерминированная среда предполагает, что каждое действие приводит к конкретному и предсказуемому результату, что упрощает процесс принятия решений. Поскольку случайность отсутствует, агенты могут с уверенностью планировать оптимальные действия. Например, судоку является детерминированной средой, так как размещение числа в определённой ячейке приводит к точному результату без изменений.

* **Пример**: В судоку каждое размещение числа даёт предсказуемый результат, что позволяет агенту планировать ходы, не учитывая неопределённость.
* **Восприятие и действия агента**: Агент действует с полной уверенностью, принимая решения исключительно на основе желаемого конечного состояния. Поскольку среда предсказуема, агент может реализовывать свои планы с высокой степенью уверенности.

Стохастическая среда, напротив, включает элементы случайности или неопределённости, что усложняет прогнозирование результатов. Например, ИИ для прогноза погоды функционирует в стохастической среде, где результаты зависят от множества непредсказуемых факторов. Агенты в таких условиях опираются на вероятностные модели или машинное обучение для оценки вероятных исходов.

* **Пример**: В прогнозировании погоды ИИ анализирует данные, такие как температура и давление, но при этом должен учитывать непредсказуемые изменения.
* **Восприятие и действия агента**: Агент использует вероятностные модели, интерпретируя свои наблюдения через оценки вероятности, чтобы направлять свои действия. Обычно действия разрабатываются с целью максимизировать ожидаемые результаты или минимизировать риски.

### 5. Одноагентная vs Многоагентная среда

Эта характеристика учитывает, действует ли агент в одиночку или взаимодействует с другими агентами.

Одноагентная среда включает только одного агента, который фокусируется исключительно на собственных целях. Роботы-пылесосы работают в одноагентной среде, так как они действуют самостоятельно, взаимодействуя только с препятствиями на своём пути.

* **Пример**: Робот-пылесос функционирует в изоляции, воспринимая препятствия и оптимизируя свой маршрут без вмешательства извне.
* **Восприятие и действия агента**: Агент может сосредоточиться исключительно на достижении своих целей, оптимизируя свою работу для повышения эффективности. Он воспринимает препятствия и прокладывает маршруты, не учитывая действия или цели других агентов.

Многоагентная среда, напротив, включает взаимодействие нескольких агентов, которые могут соревноваться или сотрудничать друг с другом. Каждый агент должен учитывать действия других, что усложняет процесс принятия решений. Например, в многопользовательской стратегической игре каждый агент (игрок) адаптируется к стратегиям соперников, часто используя принципы теории игр.

* **Пример**: В стратегической игре, такой как _StarCraft_, ИИ должен предвидеть и контрить стратегии соперников.
* **Восприятие и действия агент**а: Агент постоянно анализирует действия других агентов, динамически адаптируя свои решения. Это может включать кооперативные тактики или контрстратегии, требующие от агента повышенного внимания к действиям окружающих.

### 6. Эпизодическая vs Последовательная среда

Эпизодическая или последовательная природа среды определяет, как действия агента связаны во времени.

Эпизодическая среда позволяет агенту принимать решения в изолированных эпизодах, не беспокоясь о влиянии на будущие состояния. Каждое действие автономно, и решения не имеют долгосрочных последствий. Например, при классификации изображений каждое изображение рассматривается независимо от предыдущих или будущих, а действия агента не зависят от временных связей.

* **Пример**: Классификация изображений является эпизодической задачей; агент классифицирует каждое изображение отдельно, не принимая во внимание предыдущие или будущие изображения.
* **Восприятие и действия агента**: Агент воспринимает каждый отдельный случай независимо, фокусируясь на максимизации точности для каждой отдельной задачи без учёта долгосрочных зависимостей.

Последовательная среда требует от агента учитывать, как текущие действия повлияют на будущие состояния. В таких задачах каждое действие меняет среду и влияет на последующие ходы или стратегии. Например, в шахматах каждое перемещение фигуры меняет положение на доске, что влияет на будущие ходы и общее развитие партии.

* **Пример**: В шахматной игре каждый ход влияет на последующие, требуя разработки долгосрочной стратегии.
* **Восприятие и действия агента**: Агент планирует последовательности действий, балансируя между немедленными выгодами и долгосрочными целями. Он воспринимает не только текущее состояние, но и анализирует, как его действия могут повлиять на будущие сценарии.

### 7. Известная vs Неизвестная

Аспект знания среды касается предварительного понимания агентом законов и динамики этой среды:

Известные среды — это те, где агент имеет полное представление обо всех правилах, состояниях и динамике, что позволяет ему эффективно планировать с самого начала. Шахматная игра является примером известной среды, поскольку все правила и возможные исходы понятны, что позволяет агенту точно рассчитывать ходы.

* **Пример**: Настольная игра, такая как шашки, имеет известные правила, что позволяет агенту точно просчитать каждый возможный ход.
* **Восприятие и действия агента**: Агент воспринимает среду с полным пониманием, сосредотачиваясь на поиске оптимальной стратегии без необходимости изучения или взаимодействия для получения знаний.

Неизвестные среды, напротив, содержат скрытую динамику, которую агент должен изучить через взаимодействие. Это требует стратегий, основанных на исследовании, как, например, у автономных роботов, изучающих новые территории. Агенты в неизвестных средах используют обучение методом проб и ошибок, чтобы постепенно понять законы среды.

* **Пример**: В новой сложной видеоигре ИИ изначально может не знать, как функционирует среда, и должен экспериментировать, чтобы узнать правила и цели.
* **Восприятие и действия агента**: Агент применяет стратегии, основанные на исследовании, изучая среду через взаимодействие. Действия выбираются с целью максимизации знаний на начальных этапах, а затем переключаются на целенаправленное поведение, по мере того как агент обретает уверенность и понимание среды.

### 8. Доступная vs Недоступная

Доступность среды определяется тем, насколько агент может взаимодействовать с ней или воспринимать её:

Доступная среда — это среда, где агент имеет доступ ко всем необходимым данным для принятия полностью обоснованных решений. Онлайн-игры, где вся информация отображается на экране для игроков, являются примерами доступных сред, поскольку никакая информация не скрыта.

* **Пример**: Онлайн-платформа для шахмат с ясным и полным отображением состояния доски доступна для ИИ, так как предоставляет все необходимые данные для построения стратегии.
* **Восприятие и действия агента**: Агент полностью понимает среду, сосредотачиваясь на интерпретации доступных данных для принятия оптимальных решений. Его действия отражают процесс принятия решений на основе полной информации.

Недоступная среда ограничивает доступ к важным данным, заставляя агента делать выводы на основе недостающей информации. Например, при медицинской диагностике ИИ может не иметь полного набора данных о пациенте, что вынуждает его делать прогнозы на основе частичной информации. Агенты в недоступных средах часто используют методы выводов для восполнения пробелов в данных и принимают решения в условиях неопределённости.

* **Пример**: Система медицинской диагностики может работать с неполными данными о пациенте, что вынуждает её делать обоснованные прогнозы.
* **Восприятие и действия агента**: Агент применяет статистические модели или методы выводов для дополнения недостающей информации, выбирая осторожные или вероятностные действия для управления неопределённостью.

### Заключение

Понимание этих характеристик среды помогает создавать специализированных ИИ-агентов, которые могут оптимально воспринимать, рассуждать и действовать в зависимости от их окружения. Классификация, предложенная Расселом и Норвигом, предоставляет базовую основу для категоризации сред, что в конечном итоге способствует разработке более эффективных и интеллектуальных систем искусственного интеллекта.
