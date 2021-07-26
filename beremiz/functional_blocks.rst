Описание функциональных блоков библиотеки ИСР BEREMIZ для ПЛК BRIC 
==================================================================
Стандартные
-----------
Блок SR
~~~~~~~ 
  Данный ФБ представляет собой бистабильный SR–триггер, с доминирующим входом S (Set).

  .. figure:: images/beremiz/b-sr.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "S1", "BOOL", "Вход (доминирующий)"
    "R", "BOOL", "Сброс"
    "**Выход**", "**Тип**", "**Описание**"
    "Q1", "BOOL", "Выход становится «1», когда на вход S1 приходит «1». При переходе S1 в «0» сохраняется состояние. Выход Q1 возвращается в «0», когда вход R становится «1»"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(S1 := (*BOOL*), R := (*BOOL*));
  
    (*BOOL*) := FB.Q1;

  Блок-схема SR [1]_ 

  .. figure:: images/beremiz/sr.png
    :width: 300

  .. [1] Для использования стандартных и дополнительных функциональных блоков в языке ST необходимо перетащить необходимый из библиотеки в зону перечисления переменных и дать наименование.


Блок RS
~~~~~~~
  Данный ФБ представляет собой бистабильный RS–триггер, с доминирующим входом R (Reset).

  .. figure:: images/beremiz/b-rs.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "S", "BOOL", "Вход"
    "R1", "BOOL", "Сброс (доминирующий)"
    "**Выход**", "**Тип**", "**Описание**"
    "Q1", "BOOL", "Выход становится «1», когда вход R1 становится «0». При переходе R1 в «0» сохраняется состояние. Выход Q1 возвращается в «1», когда вход S становится «1»"
    
  Форма записи на языке ST:
  
  .. code-block:: st

    FB(S := (*BOOL*), R1 := (*BOOL*));
    
    (*BOOL*) := FB.Q1;

  Блок-схема RS

  .. figure:: images/beremiz/rs.png
          :width: 300

Блок SEMA
~~~~~~~~~
  Данный ФБ представляет собой семафор, определяющий механизм, позволяющий элементам программы иметь взаимоисключающий доступ к определенным ресурсам.

  .. figure:: images/beremiz/b-sema.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CLAIM", "BOOL", "Вход (доминирующий)"
    "RELEASE", "BOOL", "Сброс"
    "**Выход**", "**Тип**", "**Описание**"
    "BUSY", "BOOL", "Выход активируетс при CLAIM = 1 и деактивируется при RELEASE = 1"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CLAIM := (*BOOL*), RELEASE := (*BOOL*));
    
    (*BOOL*) := FB.BUSY;

  Блок-схема SEMA

  .. figure:: images/beremiz/sema.png
          :width: 300

Блок R–TRIG
~~~~~~~~~~~
  Данный ФБ представляет собой индикатор нарастания фронта, который генерирует на выходе одиночный импульс при нарастании фронта сигнала.

  .. figure:: images/beremiz/b-rtrig.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CLK", "BOOL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Выход генерирует единичный импульс, если на входе передний фронт"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CLK := (*BOOL*));
    
    (*BOOL*) := FB.Q;

  Блок-схема R-TRIG

  .. figure:: images/beremiz/r-trig.png
          :width: 300

Блок F–TRIG
~~~~~~~~~~~
  Данный ФБ представляет собой индикатор спада фронта, который генерирует на выходе одиночный импульс при спаде фронта сигнала.

  .. figure:: images/beremiz/b-f-trig.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CLK", "BOOL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Выход генерирует единичный импульс, если на входе задний фронт"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CLK := (*BOOL*));
    
    (*BOOL*) := FB.Q;

  Блок-схема F-TRIG

  .. figure:: images/beremiz/f-trig.png
          :width: 300

Блок CTU/CTU_DINT…
~~~~~~~~~~~~~~~~~~
  Данный ФБ представляет собой инкрементный счётчик.

  .. figure:: images/beremiz/b-ctu.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CU", "BOOL", "Подача импульса"
    "R", "BOOL", "Сброс"
    "PV", "ANY_INT", "Предел счета"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Принимает значение 'TRUE' когда CV >= PV"
    "CV", "ANY_INT", "Считает количество импульсов (CV = CV + 1) пока Q = 0"

  .. note::
  
    Счетчик работает только до достижения максимального значения используемого типа данных. Переполнения не происходит.
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CU := (*BOOL*), R := (*BOOL*), PV := (*ANY_INT*));
    
    (*BOOL*) := FB.Q;

    (*ANY_INT*) := FB.CV;

  Блок-схема CTU [2]_

  .. figure:: images/beremiz/ctu.png
          :width: 300

  .. [2] PV не включает тип данных USINT, SINT.

Блок CTD/CTD_DINT…
~~~~~~~~~~~~~~~~~~
  Данный ФБ представляет собой декрементный счётчик.

  .. figure:: images/beremiz/b-ctd.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CD", "BOOL", "Подача импульса"
    "LD", "BOOL", "Сброс"
    "PV", "ANY_INT", "Предел импульсов"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Принимает значение 'TRUE' когда CV = 0"
    "CV", "ANY_INT", "Считает количество импульсов (CV = CV – 1) пока Q = 0"

  .. note::
    
    Счетчик работает только до достижения минимального значения используемого типа данных. Переполнения не происходит.
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CD := (*BOOL*), LD := (*BOOL*), PV := (*ANY_INT*));
    
    (*BOOL*) := FB.Q;

    (*ANY_INT*) := FB.CV;

  Блок-схема CTD

  .. figure:: images/beremiz/ctd.png
          :width: 300

Блок CTUD/CTUD_DINT…
~~~~~~~~~~~~~~~~~~~~
  Данный ФБ представляет собой реверсивный счётчик.

  .. figure:: images/beremiz/b-ctud.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CU", "BOOL", "Подача импульса"
    "CD", "BOOL", "Подача импульса"
    "R", "BOOL", "Сброс до 0"
    "LD", "BOOL", "Сброс до PV"
    "PV", "ANY_INT", "Верхний предел импульсов"
    "**Выход**", "**Тип**", "**Описание**"
    "QU", "BOOL", "Принимает значение 'TRUE' когда CV >= PV"
    "QD", "BOOL", "Принимает значение 'TRUE' когда CV = 0"
    "CD_T", "R_TRIG", "(в существующей версии не применяется)"
    "CU_T", "R_TRIG", "(в существующей версии не применяется)"
    
  .. note::
    
    Вычитающий счетчик работает только до достижения минимального значения используемого типа данных, суммирующий счетчик работает только до достижения максимального значения используемого типа данных. Переполнения не происходит.

  Форма записи на языке ST:

  .. code-block:: st
  
  
    FB (CU := (*BOOL*), CD := (*BOOL*), R := (*BOOL*), LD := (*BOOL*), PV := (*ANY_INT*));
    
    (*BOOL*) := FB.QU;
    
    (*BOOL*) := FB.QD;
    
    (*ANY_INT*) := FB.CV;


  Блок-схема CTUD

  .. figure:: images/beremiz/ctud.png
          :width: 300

Блок TP
~~~~~~~
  Данный ФБ представляет собой повторитель импульсов и используется для генерирования импульса с заданной продолжительностью.

  .. figure:: images/beremiz/b-tp.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "BOOL", "Подача импульса"
    "PT", "TIME", "Время одного импульса"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Выход (пока ET < PT, Q = 'TRUE')"
    "ET", "TIME", "Пока IN = 1 и ET < PT идет счет времени ET"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(IN := (*BOOL*), PT := (*TIME*));
    
    (*BOOL*) := FB.Q;
    
    (*TIME*) := FB.ET;

  Блок-схема TP

  .. figure:: images/beremiz/tp.png
          :width: 250

Блок TON
~~~~~~~~
  Данный ФБ представляет собой таймер с задержкой включения. 

  .. figure:: images/beremiz/b-ton.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "BOOL", "Подача импульса"
    "PT", "TIME", "Время задержки"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Если ET = PV и IN = 1, то Q = 1, иначе Q = 0"
    "ET", "TIME", "Счетчик времени считает пока ET < PV и IN = 1"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(IN := (*BOOL*), PT := (*TIME*));
    
    (*BOOL*) := FB.Q;
    
    (*TIME*) := FB.ET;

  Блок-схема TON

  .. figure:: images/beremiz/ton.png
          :width: 250

Блок TOF
~~~~~~~~
  Данный ФБ представляет собой таймер с задержкой отключения.

  .. figure:: images/beremiz/b-tof.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "BOOL", "Подача импульса"
    "PT", "TIME", "Время задержки"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Если ET = PV и IN = 1, то Q = 1, иначе Q = 0"
    "ET", "TIME", "Счетчик времени считает пока ET < PV и IN = 1"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(IN := (*BOOL*), PT := (*TIME*));
    
    (*BOOL*) := FB.Q;
    
    (*TIME*) := FB.ET;

  Блок-схема TOF

  .. figure:: images/beremiz/tof.png
          :width: 250

Дополнительные
--------------
Блок RTC
~~~~~~~~
  Данный ФБ представляет собой часы реального времени и имеет много вариантов использования, включая добавление временных отметок, для установки даты и времени в формируемых отчетах, в аварийных сообщениях и т.д.

  .. figure:: images/beremiz/b-rtc.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "BOOL", "Переключение режима"
    "PDT", "TIME", "Время начала работы"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Индикация режима"
    "CDT", "DT", "Вывод времени"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(IN := (*BOOL*), PDT := (*DT*));
    
    (*BOOL*) := FB.Q;
    
    (*DT*) := FB.CDT;

  Блок-схема RTC

  .. figure:: images/beremiz/rtc.png
          :width: 300

Блок INTEGRAL
~~~~~~~~~~~~~
  ФБ интегрирует входное значение XIN по времени.

  .. figure:: images/beremiz/b-integral.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "RUN", "BOOL", "Включение блока"
    "R1", "BOOL", "Сброс (XOUT = X0)"
    "XIN", "REAL", "Интегрируемое значение"
    "X0", "REAL", "Начальное значение XOUT"
    "CYCLE", "TIME", "Время интегрирования"
    "**Выход**", "**Тип**", "**Описание**"
    "Q", "BOOL", "Q = 1, если R1 = 0"
    "XOUT", "REAL", "Выход (интегрирует входное значение XIN во времени) (XOUT = X0 * CYCLE (количество тактов при RUN = 1))"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(RUN := (*BOOL*), R1 := (*BOOL*), XIN := (*REAL*), X0 := (*REAL*), CYCLE := (*TIME*));
    
    (*BOOL*) := FB.Q;
    
    (*REAL*) := FB.XOUT;

  Блок-схема INTEGRAL

  .. figure:: images/beremiz/integral.png
      :width: 300

Блок DERIVATIVE
~~~~~~~~~~~~~~~
  ФБ выдаёт значение XOUT пропорционально скорости изменения входного параметра XIN.

  .. figure:: images/beremiz/b-derivative.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "RUN", "BOOL", "Включение блока"
    "XIN", "REAL", "Вход"
    "CYCLE", "TIME", "Время  дифференцирования"
    "**Выход**", "**Тип**", "**Описание**"
    "XOUT", "REAL", "Формирование сигнала пропорционально частоте изменения входа XIN"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(RUN := (*BOOL*), XIN := (*REAL*),YCLE := (*TIME*));
    
    (*REAL*) := FB.XOUT;

  Блок-схема DERIVATIVE

  .. figure:: images/beremiz/derivative.png
          :width: 200

Блок PID
~~~~~~~~
  Данный ФБ представляет собой устройство в цепи обратной связи, используемое в системах автоматического управления для формирования управляющего сигнала

  .. figure:: images/beremiz/b-pid.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "AUTO", "BOOL", "Включение блока (переход от ручного режима к автоматическому)"
    "DIR", "BOOL", "Направление регулирования ( 0 - обратное регулирование, 1 - прямое. По умолчанию стоит прямое регулирование)"
    "PV", "REAL", "Задание (автоматическое управление)"
    "SP", "REAL", "Присваиваемое значение"
    "X0", "REAL", "Задание (ручное управление)"
    "KP", "REAL", "Пропорциональный коэффициент"
    "TR", "REAL", "Интегральный коэффициент"
    "TD", "REAL", "Дифференциальный коэффициент"
    "LL", "REAL", "Нижний предел выходного сигнала XOUT. По умолчанию равно 0)"
    "LH", "REAL", "Верхний предел выходного сигнала XOUT. По умолчанию равно 1000)"
    "CYCLE", "TIME", "Время цикла"
    "**Выход**", "**Тип**", "**Описание**"
    "XOUT", "REAL", "Выход ПИД-регулятора"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(AUTO := (*BOOL*), DIR := (*BOOL*),  PV := (*REAL*),  SP := (*REAL*), X0 := (*REAL*), 
    KP := (*REAL*), TR := (*REAL*), TD := (*REAL*), LL := (*REAL*), LH := (*REAL*), 
    CYCLE := (*TIME*));
    
    (*REAL*) := FB.XOUT;

  Блок-схема PID

  .. figure:: images/beremiz/pid.png
          :width: 300

Блок RAMP
~~~~~~~~~
  .. figure:: images/beremiz/b-ramp.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "RUN", "BOOL", "Включение блока"
    "X0", "REAL", "Начало отсчета"
    "X1", "REAL", "Конечное значение"
    "TR", "TIME", "Время перехода"
    "CYCLE", "TIME", "Время интегрирования"
    "**Выход**", "**Тип**", "**Описание**"
    "BUSY", "BOOL", "BUSY = 1, если значения XOUT меняются"
    "XOUT", "REAL", "Если RUN = 1, то пока XOUT < X1 XOUT = X1*CYCLE*t/TR + X0 иначе XOUT = X0"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(RUN := (*BOOL*), X0 := (*REAL*), X1 := (*REAL*), TR := (*TIME*), CYCLE := (*TIME*));
    
    (*BOOL*) := FB.BUSY;
    
    (*REAL*) := FB.XOUT;

  Блок-схема RAMP

  .. figure:: images/beremiz/ramp.png
        :width: 300

Блок READ_DI
~~~~~~~~~~~~
  ФБ READ_DI предоставляет данные с дискретных входных каналов ПЛК записанного в одну переменную типа (UDINT) при этом 0 бит соответствует DI_0, а 15 бит DI_15.

  .. figure:: images/beremiz/b-read_di.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_OUT", "UDINT", "Состояние дискретных входов ПЛК BRIC"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*UDINT*) := FB.DI_OUT;


Блок READ_DI_CNT
~~~~~~~~~~~~~~~~
  ФБ READ_DI_CNT предоставляет значение счетчика дискретного входного канала ПЛК номер, которого прописан на входе блока.

  .. figure:: images/beremiz/b-read_di_cnt.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_NUMBER", "UINT", "Номер исследуемого дискретного входа"
    "**Выход**", "**Тип**", "**Описание**"
    "DI_CNT_VALUE", "ULINT", "Значение счетчика дискретного входа"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DI_NUMBER := (*UINT*));
    
    (*ULINT*) := FB.DI_CNT_VALUE;

Блок READ_DI_FREQ
~~~~~~~~~~~~~~~~~
  ФБ READ_DI_FREQ предоставляет значение частоты дискретного входного канала ПЛК, номер которого прописан на входе блока.

  .. figure:: images/beremiz/b-read_di_freq.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_NUMBER", "UINT", "Номер исследуемого дискретного входа"
    "**Выход**", "**Тип**", "**Описание**"
    "DI_FREQ_VALUE", "ULINT", "Значение частоты переключения дискретного входа"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DI_NUMBER := (*UINT*));
    
    (*ULINT*) := FB.DI_FREQ_VALUE;

Блок READ_AI
~~~~~~~~~~~~
  ФБ READ_AI указывает значение аналогового входного канала ПЛК номер, которого прописан на входе блока.

  .. figure:: images/beremiz/b-read_ai.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "AI_NUMBER", "UINT", "Номер канала"
    "**Выход**", "**Тип**", "**Описание**"
    "AI_VALUE", "UINT", "Показание аналогового канала по шкале 0 – 16383, которая соответствует шкале по принимаемого унифицированного сигнала"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(AI_NUMBER := (*UINT*));
    
    (*UINT*) := FB.AI_VALUE;

Блок READ_DO
~~~~~~~~~~~~
  ФБ READ_DO предоставляет информацию о состоянии дискретных выходов.

  .. figure:: images/beremiz/b-read_do.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DO_OUT", "USINT", "Чтение значения состояния дискретных выходов"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*USINT*) := FB.DO_OUT;

Блок READ_RESET
~~~~~~~~~~~~~~~
  ФБ READ_RESET предоставляет информацию о количестве перезагрузок с момента обновления ОС ПЛК и причине последней перезагрузки.

  .. figure:: images/beremiz/b-read_reset.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "RESET_NUM", "UINT", "Количество перезагрузок с момента обновления ОС ПЛК"
    "LAST_RESET", "UINT", "Код причины последней перезагрузки"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*UINT*) := FB.RESET_NUM;

    (*UINT*) := FB.LAST_RESET;

Блок READ_PWR
~~~~~~~~~~~~~
  ФБ READ_PWR предоставляет информацию о входном напряжении питания и напряжение батареи ПЛК.

  .. figure:: images/beremiz/b-read_pwr.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "V_PWR", "REAL", "Входное напряжение ПЛК"
    "V_BAT", "REAL", "Напряжение батареи ПЛК"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*REAL*) := FB.V_PWR;

    (*REAL*) := FB.V_BAT;

Блок READ_INTERNAL_TEMP
~~~~~~~~~~~~~~~~~~~~~~~
  ФБ READ_INTERNAL_TEMP предоставляет информацию температуре микропроцессора ПЛК BRIC.

  .. figure:: images/beremiz/b-read_int_temp.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "INTERNAL_TEMP_OUT", "REAL", "Показание температуры микропроцессора в ПЛК"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*REAL*) := FB.INTERNAL_TEMP_OUT;

Блок READ_SYS_TICK_COUNTER
~~~~~~~~~~~~~~~~~~~~~~~~~~
  ФБ READ_SYS_TICK_COUNTER предоставляет информацию о времени работы ПЛК с момента последней перезагрузки указанной в мс.

  .. figure:: images/beremiz/b-read_sys.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "SYS_TICK_COUNTER_VALUE", "ULINT", "Чтение времени выполнения с момента последнего сброса в мс."
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*ULINT*) := FB.SYS_TICK_COUNTER_VALUE;

Блок WRITE_MDB_ADRESS
~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_MDB_ADRESS позволяет установить Modbus адрес ПЛК (от 1 до 247).

  .. figure:: images/beremiz/b-write_mdb.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "MDB_ADDR", "UINT", "Установление адреса по протоколу Modbus в ПЛК"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(MDB_ADDR := (*UINT*));
   
Блок WRITE_UART_SETS
~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_UART_SETS предоставляет изменять параметры UART каналов записанных в формате U16.

  .. figure:: images/beremiz/b-write_uart.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "MESO_UART", "UINT", "Запись параметров настройки интерфейса на канале Mesonin (доступ к порту только при вскрытии крышки ПЛК)"
    "SET_RS_485_2", "UINT", "Запись параметров настройки интерфейса на канале RS–485–1"
    "SET_RS_232", "UINT", "Запись параметров настройки интерфейса на канале RS–232"
    "SET_RS_485_1", "UINT", "Запись параметров настройки интерфейса на канале RS–485–1"
    "SET_RS_485_IMMO", "UINT", "Запись параметров настройки интерфейса на межмодульной шине"
    "SET_HART", "UINT", "Запись параметров HART интерфейса на каналах AI"
    
  Форма записи на языке ST:

  .. code-block:: st
  
    FB(MESO_UART := (*UINT*), SET_RS_485_2 := (*UINT*), SET_RS_232 := (*UINT*), 
    SET_RS_485_1 := (*UINT*), SET_RS_485_IMMO := (*UINT*), SET_HART := (*UINT*));

  .. figure:: images/beremiz/26.png
      :width: 300
      :align: center

      *Структура изменения параметров UART каналов*
    
Блок WRITE_CH_TIMEOUT
~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_CH_TIMEOUT изменяет время задержки UART каналов.

  .. figure:: images/beremiz/b-write_ch_time.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "CH_NUMBER", "USINT", "Номер канала"
    "CHANNEL_TIMEOUT", "UDINT", "Задержка"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(CH_NUMBER:= (*USINT*), CHANNEL_TIMEOUT:= (*UDINT*));
  
  .. csv-table::
    :header: "Номер канала","Наименование UART канала"
    :widths: 20, 30

    "0", "MESO_UART_UART"
    "1", "RS_485_2_UART"
    "2", "RS_232_UART"
    "4", "RS_485_1_UART"
    "5", "RS_485_IMMO_UART"
    "6", "HART_UART"
    
Блок WRITE_DI_NOISE_FLTR_10US
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_DI_NOISE_FLTR_10US для указанного дискретного входа задается период нечувствительности импульса в диапазоне от 0 до 65512, при этом считается в десятках мс.

  .. figure:: images/beremiz/b-write_di_noise.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_NUMBER", "UINT", "Номер канала дискретного входа ПЛК BRIC"
    "DI_NOISE_FLTR_VALUE_10US", "UINT", "Период, за который счетчик обнаруживает не более 1 импульса"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DI_NUMBER := (*UINT*), DI_NOISE_FLTR_VALUE_10US := (*UINT*));

Блок WRITE_DI_PULSELESS
~~~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_DI_PULSELESS для указанного дискретного входа задает период, за который ведется подсчет импульсов для расчета частоты

  .. figure:: images/beremiz/b-write_di_puls.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_NUMBER", "UINT", "Номер канала дискретного входа ПЛК BRIC"
    "DI_PULSELESS_VALUE", "UINT", "Период, относительно которого рассчитывается частота канала"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DI_NUMBER := (*UINT*), DI_PULSELESS_VALUE := (*UINT*));

Блок WRITE_DI_MODE
~~~~~~~~~~~~~~~~~~
  ФБ WRITE_DI_MODE для указанного дискретного входа обозначает подключенные опции (0– не подключены, 1– подключен счетчик импульсов, 2– подключен расчет частоты дискретного входа, 3– подключен счетчик импульсов и расчет частоты дискретного входа).

  .. figure:: images/beremiz/b-write_di_mode.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DI_NUMBER", "UINT", "Номер канала дискретного входа ПЛК BRIC"
    "DI_MODE_VALUE", "UINT", "Код подключенных функций данного канала"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DI_NUMBER := (*UINT*), DI_MODE_VALUE := (*UINT*));

Блок WRITE_DO
~~~~~~~~~~~~~
  ФБ WRITE_DO согласно маске DO_MASK и подаче напряжений на соответствующие дискретные выхода DO_VALUE (выхода прописываются побитово).

  .. figure:: images/beremiz/b-write_do.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DO_VALUE", "UINT", "Запись значения состояния дискретных выходов"
    "DO_MASK", "UINT", "Запись маски, разрешающей изменять состояние дискретных выходов"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DO_VALUE := (*UINT*), DO_MASK := (*UINT*));

Блок WRITE_DO_SC
~~~~~~~~~~~~~~~~
  ФБ WRITE_DO_SC устанавливает номера дискретных выходов, на которых включить программную защиту от КЗ и дискретные выхода на которых сработала программная защита от КЗ.

  .. figure:: images/beremiz/b-write_do_sc.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DO_SC_FLAG", "UINT", "Запись перечисления дискретных выходов, на которых сработала аппаратная защита от короткого замыкания (использовать только возможность сброса)"
    "DO_SC_EN", "UINT", "Запись перечисления дискретных выходов, на которых сработала программная защита от короткого замыкания (использовать только возможность сброса)"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DO_SC_FLAG := (*UINT*), DO_SC_EN := (*UINT*));

Блок WRITE_DO_PWM_FREQ
~~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_DO_PWM устанавливает ШИМ дискретных выходов в диапазоне от 0 до 10000 Гц.

  .. figure:: images/beremiz/b-write_do_pwm.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DO_PWM_FREQ", "UINT", "Частота ШИМ подаваемая на каналы дискретных выходов, Гц"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DO_PWM_FREQ := (*UINT*));

Блок WRITE_DO_PWM_CTRL
~~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_DO_PWM_CTRL устанавливает для выбранного дискретного выхода скважность сигнала указывается диапазон от 0 до 127 соответствующий от 0 до 100%.

  .. figure:: images/beremiz/b-write_do_pwm_ctrl.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "DO_NUMBER", "USINT", "Номер канала дискретного выхода ПЛК BRIC"
    "DO_PWM_CTRL", "UINT", "Указание скважности канала"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB(DO_NUMBER := (*UINT*), DO_PWM_CTRL := (*UINT*));

Блок STRUCT_REAL_TIME
~~~~~~~~~~~~~~~~~~~~~
  ФБ STRUCT_REAL_TIME позволяет считать время с ПЛК.

  .. figure:: images/beremiz/b-struct_real.png
    :align: left

  .. csv-table::
    :header: "Выход", "Тип", "Описание"
    :widths: 1, 1, 10

    "HOUR_TIME", "USINT", "Текущий час реального времени внутреннего определения"
    "MINUTE_TIME", "USINT", "Текущая минута"
    "SEC_TIME", "USINT", "Текущая секунда"
    "SUB_SEC_TIME", "USINT", "Текущая миллисекунда"
    "WEEK_DAY_TIME", "USINT", "Текущий день недели"
    "MONTH_TIME", "USINT", "Текущий месяц"
    "DATE_TIME", "USINT", "Текущий день месяца"
    "YEAR_TIME", "USINT", "Текущий год"
    "YEAR_DAY_TIME", "UINT", "Текущий день года"

  Форма записи на языке ST:

  .. code-block:: st
  
    FB();
    
    (*USINT*) := FB.HOUR_TIME;
    
    (*USINT*) := FB.MINUTE_TIME;
    
    (*USINT*) := FB.SEC_TIME;
    
    (*USINT*) := FB.SUB_SEC_TIME;
    
    (*USINT*) := FB.WEEK_DAY_TIME;
    
    (*USINT*) := FB.MONTH_TIME;
    
    (*USINT*) := FB.DATE_TIME;
    
    (*USINT*) := FB.YEAR_TIME;
    
    (*UINT*) := FB.YEAR_DAY_TIME;

Блок UNIX_TIME
~~~~~~~~~~~~~~
  ФБ UNIX_TIME устанавливает время на ПЛК (время указывается в мс.).

  .. figure:: images/beremiz/b-unix_time.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "UNIX_TIME_WRITE", "UDINT", "Запись Unix time на ПЛК"
    "**Выход**", "**Тип**", "**Описание**"
    "UNIX_TIME_READ", "UDINT", "Unix time"
    "UNIX_TIME_WRITED", "UDINT", "Последние записанное значени UNIX_TIME_WRITE"
  
  .. note::
    Время задается в контроллер только по изменению UNIX_TIME_WRITE, начальное значение для сравнения 0.
    ФБ можно использовать для чтения UNIX time в нескольких местах программы, для этого UNIX_TIME_WRITE
    оставляем нулевым.

  Форма записи на языке ST:

  .. code-block:: st
  
  
    FB(UNIX_TIME_WRITE := (*UDINT*));
    
    (*UDINT*) := FB.UNIX_TIME_READ; 
    
    (*UDINT*) := FB.UNIX_TIME_WRITED;

Блок WRITE_STRUCT_TIME
~~~~~~~~~~~~~~~~~~~~~~
  ФБ WRITE_STRUCT_TIME устанавливает время на ПЛК, выхода функционального блока используются для проверки записанного времени.

  .. figure:: images/beremiz/b-write_struct_time.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "SEC_IN_MIN", "USINT", "Ввод секунд"
    "MINUTE_IN_HOUR", "USINT", "Ввод минут"
    "HOUR_IN_DAY", "USINT", "Ввод часов"
    "DATE_IN_MONTH", "USINT", "Ввод дня месяца"
    "MONTH_IN_YEAR", "USINT", "Ввод месяца года"
    "YEAR_SINCE_2000", "USINT", "Ввод года"
    "**Выход**", "**Тип**", "**Описание**"
    "SEC_IN_MIN_WRITED", "USINT", "Вывод присвоенного значения"
    "MINUTE_IN_HOUR_WRITED", "USINT", "Вывод присвоенного значения"
    "HOUR_IN_DAY_WRITED", "USINT", "Вывод присвоенного значения"
    "DATE_IN_MONTH_WRITED", "USINT", "Вывод присвоенного значения"
    "MONTH_IN_YEAR_WRITED", "USINT", "Вывод присвоенного значения"
    "YEAR_SINCE_2000_WRITED", "USINT", "Вывод присвоенного значения"
    
  Форма записи на языке ST:

  .. code-block:: st  
  
    FB(SEC_IN_MIN := (*USINT*), MINUTE_IN_HOUR := (*USINT*), HOUR_IN_DAY := (*USINT*), 
    DATE_IN_MONTH := (*USINT*), MONTH_IN_YEAR := (*USINT*), YEAR_SINCE_2000 := (*USINT*));
    
    (*USINT*) := FB.SEC_IN_MIN_WRITED; 
    
    (*USINT*) := FB.MINUTE_IN_HOUR_WRITED; 
    
    (*USINT*) := FB.HOUR_IN_DAY_WRITED; 
    
    (*USINT*) := FB.DATE_IN_MONTH_WRITED; 
    
    (*USINT*) := FB.MONTH_IN_YEAR_WRITED; 
    
    (*USINT*) := FB.YEAR_SINCE_2000_WRITED;

Блок PARSING_UINT
~~~~~~~~~~~~~~~~~
  ФБ PARSING_UINT выводит значение входной переменной UINT в битах.

  .. figure:: images/beremiz/fb10.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN_VAL", "UINT", "Обрабатываемая переменная"
    "**Выход**", "**Тип**", "**Описание**"
    "BIT_1", "BOOL", "Вывод значения первого бита"
    "BIT_2", "BOOL", "Вывод значения второго бита"
    "BIT_3", "BOOL", "Вывод значения третьего бита"
    "BIT_4", "BOOL", "Вывод значения четвертого бита"
    "BIT_5", "BOOL", "Вывод значения пятого бита"
    "BIT_6", "BOOL", "Вывод значения шестого бита"
    "BIT_7", "BOOL", "Вывод значения седьмого бита"
    "BIT_8", "BOOL", "Вывод значения восьмого бита"
    "BIT_9", "BOOL", "Вывод значения девятого бита"
    "BIT_10", "BOOL", "Вывод значения десятого бита"
    "BIT_11", "BOOL", "Вывод значения одиннацатого бита"
    "BIT_12", "BOOL", "Вывод значения двенадцатого бита"
    "BIT_13", "BOOL", "Вывод значения тринадцатого бита"
    "BIT_14", "BOOL", "Вывод значения четырнадцатого бита"
    "BIT_15", "BOOL", "Вывод значения пятнадцатого бита"
    "BIT_16", "BOOL", "Вывод значения шестнадцатого бита"
    
  Форма записи на языке ST:

  .. code-block:: st  
  
    FB(IN_VAL := (*UINT*));
    
    (*BOOL*) := FB.BIT_1; 
    
    (*BOOL*) := FB.BIT_2; 
    
    (*BOOL*) := FB.BIT_3; 
    
    (*BOOL*) := FB.BIT_4; 
    
    (*BOOL*) := FB.BIT_5

    (*BOOL*) := FB.BIT_6;

    (*BOOL*) := FB.BIT_7; 
    
    (*BOOL*) := FB.BIT_8; 
    
    (*BOOL*) := FB.BIT_9; 
    
    (*BOOL*) := FB.BIT_10; 
    
    (*BOOL*) := FB.BIT_11

    (*BOOL*) := FB.BIT_12;

    (*BOOL*) := FB.BIT_13; 
    
    (*BOOL*) := FB.BIT_14; 
    
    (*BOOL*) := FB.BIT_15; 
    
    (*BOOL*) := FB.BIT_16; 

Блок PARSING_USINT
~~~~~~~~~~~~~~~~~~
  ФБ PARSING_USINT выводит значение входной переменной UINT в битах.

  .. figure:: images/beremiz/fb11.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN_VAL", "USINT", "Обрабатываемая переменная"
    "**Выход**", "**Тип**", "**Описание**"
    "BIT_1", "BOOL", "Вывод значения первого бита"
    "BIT_2", "BOOL", "Вывод значения второго бита"
    "BIT_3", "BOOL", "Вывод значения третьего бита"
    "BIT_4", "BOOL", "Вывод значения четвертого бита"
    "BIT_5", "BOOL", "Вывод значения пятого бита"
    "BIT_6", "BOOL", "Вывод значения шестого бита"
    "BIT_7", "BOOL", "Вывод значения седьмого бита"
    "BIT_8", "BOOL", "Вывод значения восьмого бита"
    
  Форма записи на языке ST:

  .. code-block:: st  
  
    FB(IN_VAL := (*USINT*));
    
    (*BOOL*) := FB.BIT_1; 
    
    (*BOOL*) := FB.BIT_2; 
    
    (*BOOL*) := FB.BIT_3; 
    
    (*BOOL*) := FB.BIT_4; 
    
    (*BOOL*) := FB.BIT_5

    (*BOOL*) := FB.BIT_6;

    (*BOOL*) := FB.BIT_7; 
    
    (*BOOL*) := FB.BIT_8; 

Преобразование типов
--------------------
Блок \*_TO_*\
~~~~~~~~~~~~~
  Данный ФБ предназначен для всех возможных и корректных, согласно стандарту IEC 61131–3, преобразований между типами данных.

  .. figure:: images/beremiz/b-to.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "…", "Преобразуемая информация"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "…", "Преобразованная информация"
  
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := *_TO_*(*ANY*);


  .. csv-table:: Перечисление блоков преобразования типов
    :header: "Тип", "Описание"
    :widths: 10, 20

    "REAL_TO_*\", "Преобразование REAL в остальные типы данных"
    "SINT_TO_*\", "Преобразование SINT в остальные типы данных"
    "LINT_TO_*\", "Преобразование LINT в остальные типы данных"
    "DINT_TO_*\", "Преобразование DINT в остальные типы данных"
    "DATE_TO_*\", "Преобразование DATE в остальные типы данных"
    "DWORD_TO_*\", "Преобразование DWORD в остальные типы данных"
    "DT_TO_*\", "Преобразование DT в остальные типы данных"
    "TOD_TO_*\", "Преобразование TOD в остальные типы данных"
    "UDINT_TO_*\", "Преобразование UDINT в остальные типы данных"
    "WORD_TO_*\", "Преобразование WORD в остальные типы данных"
    "STRING_TO_*\", "Преобразование STRING в остальные типы данных"
    "LWORD_TO_*\", "Преобразование LWORD в остальные типы данных"
    "UINT_TO_*\", "Преобразование UINT в остальные типы данных"
    "LREAL_TO_*\", "Преобразование LREAL в остальные типы данных"
    "BYTE_TO_*\", "Преобразование BYTE в остальные типы данных"
    "USINT_TO_*\", "Преобразование USINT в остальные типы данных"
    "ULINT_TO_*\", "Преобразование ULINT в остальные типы данных"
    "BOOL_TO_*\", "Преобразование BOOL в остальные типы данных"
    "TIME_TO_*\", "Преобразование TIME в остальные типы данных"
    "INT_TO_*\", "Преобразование INT в остальные типы данных"


Математические функции
----------------------
Блок ABS
~~~~~~~~
  Данный ФБ возвращает в OUT модуль входного числа IN.

  .. figure:: images/beremiz/b-abs.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_NUM", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_NUM", "Выход (модуль числа)"
    
  Форма записи на языке ST:
  
  .. code-block:: st 

    (*ANY_NUM*) := ABS((*ANY_NUM*));

Блок SQRT
~~~~~~~~~
  Данный ФБ возвращает в OUT квадратный корень входного числа IN.

  .. figure:: images/beremiz/b-sqrt.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (корень числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := SQRT((*ANY_REAL*));
  
Блок LN
~~~~~~~
  Данный ФБ возвращает в OUT значение натурального логарифма от IN.

  .. figure:: images/beremiz/b-ln.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (натуральный логарифм числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := LN((*ANY_REAL*));
  
  
Блок LOG
~~~~~~~~
  Данный ФБ возвращает в OUT значение логарифма по основанию 10 от IN.

  .. figure:: images/beremiz/b-log.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (логарифм числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := LOG((*ANY_REAL*));
  

Блок EXP
~~~~~~~~
  Данный ФБ возвращает в OUT значение экспоненты, возведённой в степень IN.

  .. figure:: images/beremiz/b-exp.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (экспонента числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := EXP((*ANY_REAL*));
  

Блок SIN
~~~~~~~~
  Данный ФБ возвращает в OUT значение синуса IN.

  .. figure:: images/beremiz/b-sin.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (синус числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := SIN((*ANY_REAL*));
  

Блок COS
~~~~~~~~
  Данный ФБ возвращает в OUT значение косинуса IN.

  .. figure:: images/beremiz/b-cos.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (косинус числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := COS((*ANY_REAL*));
  

Блок TAN
~~~~~~~~
  Данный ФБ возвращает в OUT значение тангенса IN.

  .. figure:: images/beremiz/b-tan.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (тангенс числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := TAN((*ANY_REAL*));
  

Блок ASIN
~~~~~~~~~
  Данный ФБ возвращает в OUT значение арксинуса IN.

  .. figure:: images/beremiz/b-asin.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (арксинус числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := ASIN((*ANY_REAL*));
  
Блок ACOS
~~~~~~~~~
  Данный ФБ возвращает в OUT значение арккосинуса IN.

  .. figure:: images/beremiz/b-acos.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (арккосинус числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := ACOS((*ANY_REAL*));
  
  *Блок-схема ACOS*

Блок ATAN
~~~~~~~~~
  Данный ФБ возвращает в OUT значение арктангенса IN.

  .. figure:: images/beremiz/b-atan.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN", "ANY_REAL", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (арктангенс числа)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := ATAN((*ANY_REAL*));

Арифметические
--------------
Блок ADD
~~~~~~~~
  Данный ФБ возвращает в OUT результат сложения IN1 и IN2.

  .. figure:: images/beremiz/b-add.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN1", "ANY_NUM", "Вход"
    "IN2", "ANY_NUM", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_NUM", "Выход (сложение)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_NUM*) := ADD((*ANY_NUM*), (*ANY_NUM*));
  
Блок MUL
~~~~~~~~
  Данный ФБ возвращает в OUT результат умножения IN1 и IN2.

  .. figure:: images/beremiz/b-mul.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1, 10

    "IN1", "ANY_NUM", "Вход"
    "IN2", "ANY_NUM", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_NUM", "Выход (умножение)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_NUM*) := MUL((*ANY_NUM*), (*ANY_NUM*));
  
Блок SUB
~~~~~~~~
  Данный ФБ возвращает в OUT результат вычитания из IN1 значения IN2.

  .. figure:: images/beremiz/b-sub.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_NUM", "Вход"
    "IN2", "ANY_NUM", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_NUM", "Выход (разность)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_NUM*) := SUB((*ANY_NUM*), (*ANY_NUM*));
  
Блок DIV
~~~~~~~~
  Данный ФБ возвращает в OUT результат деления IN1 на IN2.

  .. figure:: images/beremiz/b-div.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_NUM", "Вход (делитель)"
    "IN2", "ANY_NUM", "Вход (делитель)"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_NUM", "Выход (деление)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_NUM*) := DIV((*ANY_NUM*), (*ANY_NUM*));
  
Блок MOD
~~~~~~~~
  Данный ФБ возвращает в OUT остаток от деления IN1 на IN2.

  .. figure:: images/beremiz/b-mod.png
    :alig: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_INT", "Вход (делимое)"
    "IN2", "ANY_INT", "Вход (делитель)"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_INT", "Выход (остаток от деления)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_INT*) := MOD((*ANY_INT*), (*ANY_INT*));
  
Блок EXPT
~~~~~~~~~
  Данный ФБ возвращает в OUT значение IN1 возведённое в степень IN2.

  .. figure:: images/beremiz/b-expt.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_REAL", "Вход"
    "IN2", "ANY_NUM", "Вход (степень)"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_REAL", "Выход (число в степени)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := EXPT((*ANY_REAL*), (*ANY_NUM*));

Блок MOVE
~~~~~~~~~
  Данный ФБ возвращает в OUT значение IN.

  .. figure:: images/beremiz/b-move.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN", "ANY", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход (присваивание)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_REAL*) := MOVE((*ANY*));

Сдвиговые операции
------------------
Блок SHL
~~~~~~~~
  Данный ФБ возвращает в OUT арифметический сдвиг аргумента IN на N бит влево с заполнением битов справа нулями.

  .. figure:: images/beremiz/b-shl.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_INT", "Количество сдвигов влево"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := SHL((*ANY_BIT*), (*ANY_INT*));
  
  *Блок-схема SHL* [3]_ 

  .. figure:: images/beremiz/shl.png
        :width: 200

  .. [3] IN1 Тип переменной выхода должен соответствовать типу переменной входа. У OUT нет возможности использования переменных типа BOOL.

Блок SHR
~~~~~~~~
  Данный ФБ возвращает в OUT арифметический сдвиг аргумента IN на N бит вправо с заполнением битов слева нулями.

  .. figure:: images/beremiz/b-shr.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_INT", "Количество сдвигов вправо"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := SHR((*ANY_BIT*), (*ANY_INT*));
  
  *Блок-схема SHR* [4]_ 

  .. figure:: images/beremiz/shr.png
        :width: 200

  .. [4] IN1 Тип переменной выхода должен соответствовать типу переменной входа. У OUT нет возможности использования переменных типа BOOL.

Блок ROR
~~~~~~~~
  Данный ФБ возвращает в OUT циклический сдвиг аргумента IN на N бит влево.

  .. figure:: images/beremiz/b-ror.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_INT", "Количество сдвигов вправо"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := ROR((*ANY_BIT*), (*ANY_INT*));
  
  *Блок-схема ROR* [5]_ 

  .. figure:: images/beremiz/ror.png
        :width: 200

  .. [5] IN1 Тип переменной выхода должен соответствовать типу переменной входа. У OUT нет возможности использования переменных типа BOOL.

Блок ROL
~~~~~~~~
  Данный ФБ возвращает в OUT циклический сдвиг аргумента IN на N бит вправо.

  .. figure:: images/beremiz/b-rol.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_INT", "Количество сдвигов влево"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := ROL((*ANY_BIT*), (*ANY_INT*));
  
  *Блок-схема ROL* [6]_ 

  .. figure:: images/beremiz/rol.png
        :width: 200

  .. [6] IN1 Тип переменной выхода должен соответствовать типу переменной входа. У OUT нет возможности использования переменных типа BOOL.

Битовые операции
----------------
Блок AND
~~~~~~~~
  Данный ФБ представляет собой организацию «логического И» для всех входных аргументов IN1…INn.

  .. figure:: images/beremiz/b-and.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_BIT", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := AND((*ANY_BIT*), (*ANY_BIT*));
  
  *Блок-схема AND*

  .. figure:: images/beremiz/and.png
        :width: 300

Блок OR
~~~~~~~
  Данный ФБ представляет собой организацию «логического ИЛИ» для всех входных аргументов IN1…INn.

  .. figure:: images/beremiz/b-or.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_BIT", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := OR((*ANY_BIT*), (*ANY_BIT*));
  
  *Блок-схема OR*

  .. figure:: images/beremiz/or.png
        :width: 300

Блок XOR
~~~~~~~~
  Данный ФБ представляет собой организацию «логического исключающего ИЛИ» для всех входных аргументов IN1…INn.

  .. figure:: images/beremiz/b-xor.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY_BIT", "Вход"
    "IN2", "ANY_BIT", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := XOR((*ANY_BIT*), (*ANY_BIT*));
  
  *Блок-схема XOR*

  .. figure:: images/beremiz/xor.png
        :width: 300

Блок NOT
~~~~~~~~
  Данный ФБ представляет собой организацию «логической инверсии» для входного аргумента IN.

  .. figure:: images/beremiz/b-not.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN", "ANY_BIT", "Вход"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY_BIT", "Выход (инверсия)"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY_BIT*) := NOT((*ANY_BIT*), (*ANY_BIT*));
  
Выбор
-----
Блок SEL
~~~~~~~~
  Данный ФБ возвращает в OUT один из двух аргументов IN1 или IN2 в зависимости от значения аргумента G.

  .. figure:: images/beremiz/b-sel.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "G", "BOOL, "Номер входа"
    "IN0", "ANY", "Вход 1"
    "IN1", "ANY", "Вход 2"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := SEL((*BOOL*), (*ANY*), (*ANY*));
  
  *Блок-схема SEL*

  .. figure:: images/beremiz/sel.png
        :width: 300

Блок MAX
~~~~~~~~
  Данный ФБ возвращает в OUT максимум из входных аргументов IN1 и IN2.

  .. figure:: images/beremiz/b-max.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "…", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := MAX((*ANY*), (*ANY*));
  
  *Блок-схема MAX*

  .. figure:: images/beremiz/max.png
        :width: 300

Блок MIN
~~~~~~~~
  Данный ФБ возвращает в OUT минимум из входных аргументов IN1 и IN2.

  .. figure:: images/beremiz/b-min.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "…", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := MIN((*ANY*), (*ANY*));
  
  *Блок-схема MIN*

  .. figure:: images/beremiz/min.png
        :width: 300

Блок LIMIT
~~~~~~~~~~
  Данный ФБ возвращает в OUT значение входного аргумента IN, в случае превышения им значения MX – в OUT возвращается MX, в случае если IN меньше MN – в OUT возвращается MN.

  .. figure:: images/beremiz/b-limit.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "MN", "ANY", "Вход (нижний предел)"
    "IN", "ANY", "Вход"
    "MX", "ANY", "Вход (верхний предел)"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := LIMIT((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема LIMIT*

  .. figure:: images/beremiz/limit.png
        :width: 300

Блок MUX
~~~~~~~~
  Данный ФБ возвращает в OUT значение на входе IN(K), в зависимости от входного K. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-mux.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "K", "ANY_INT", "Переключатель"
    "IN0", "ANY", "Вход 1"
    "IN1", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "ANY", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*ANY*) := MUX((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема MUX*

  .. figure:: images/beremiz/mux.png
        :width: 300

Сравнение
---------
Блок GT
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 > IN2) & (IN2 > IN3) & … (INn–1 > INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-gt.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := GT((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема GT*

  .. figure:: images/beremiz/gt.png
        :width: 300

Блок GE
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 >= IN2) & (IN2 >= IN3) &… (INn–1 >= INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-ge.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := GE((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема GE*

  .. figure:: images/beremiz/ge.png
     :width: 300

Блок EQ
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 = IN2) & (IN2 = IN3) & … (INn–1 = INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-eq.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := EQ((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема EQ*

  .. figure:: images/beremiz/eq.png
     :width: 300

Блок LT
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 < IN2) & (IN2 < IN3) & … (INn–1 < INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-lt.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := LT((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема LT*

  .. figure:: images/beremiz/lt.png
     :width: 300

Блок LE
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 <= IN2) & (IN2 <= IN3) & … (INn–1 <= INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-le.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := LE((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема LE*

  .. figure:: images/beremiz/le.png
     :width: 300

Блок NE
~~~~~~~
  Данный ФБ сравнивает все входные аргументы и выдаёт на выходе OUT значение True, если выполнится следующее условие: (IN1 <> IN2) & (IN2 <> IN3) &… (INn–1 <> INn), в противном случае в OUT выдаётся False. Количество входов IN(n) изменяемое – от 2 до 20. По умолчанию 2.

  .. figure:: images/beremiz/b-le.png
    :align: left

  .. csv-table::
    :header: "Вход", "Тип", "Описание"
    :widths: 1, 1 , 10

    "IN1", "ANY", "Вход 1"
    "IN2", "ANY", "Вход 2"
    "…", "ANY", "Вход …"
    "**Выход**", "**Тип**", "**Описание**"
    "OUT", "BOOL", "Выход"
    
  Форма записи на языке ST:

  .. code-block:: st 
  
    (*BOOL*) := NE((*ANY*), (*ANY*), (*ANY*));
  
  *Блок-схема NE*

  .. figure:: images/beremiz/ne.png
     :width: 200
