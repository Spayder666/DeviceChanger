Devices Changer

Комплекс для управляемой подмены идентификаторов, сборки, сети, локации и окружения на rooted Android. Ожидаемые сценарии: приватность, тестирование приложений и прошивок, отладка API «под другим железом».

Пакет приложения: com.motorola.backup
Имя в лаунчере: Devices Changer
Magisk / KernelSU модуль: Cheetah_Prop (/data/adb/modules/Cheetah_Prop)
LSPosed-модуль в менеджере: Device ID Spoofer

minSdk
30 (Android 11)

targetSdk
33

compileSdk
36

Языки UI
English, Русский, 简体中文

Сайт: deviceschanger.org
Канал: t.me/DeviceChanger
Бот лицензий: @DeviceChanger_bot

Как это устроено
Один APK — менеджер. Подмена идёт несколькими слоями; без нужного слоя часть значений «не везде» меняется.

Слой
Что делает
Приложение

UI, лицензия, запись конфигов, установка/обновление Cheetah_Prop, экспорт KPM, обои при первом запуске

Magisk / KernelSU модуль Cheetah_Prop

post-fs-data / service.sh: свойства, зеркала конфигов, device.kernel, автозагрузка KPM

LSPosed
Java/Kotlin хуки в выбранных процессах (в т.ч. system_server, если System Framework в scope)

Zygisk-модули (опционально)
Hide (список приложений), MediaDrm, VPN Hide, камера StreamDC
KernelPatch KPM (опционально, APatch / KPatch)
native: карты/пути (susmap), TUN (tunhide), uname() syscall (unamehide)
Хуки читают актуальный конфиг из модуля и публичных зеркал


Требования
Root: Magisk, KernelSU или совместимый менеджер
LSPosed. Классический Xposed не поддерживается
Разрешение root приложению при первом запуске
Активный код доступа (лицензия) — без него доступны только Home и Help
Для GPS / SIM / вышек / LocationManager: модуль включён в LSPosed и в scope есть целевые приложения плюс System Framework (android)
KernelSU: скрытие путей модуля ломает чтение зеркал. Проект дублирует данные в модуль и публичные зеркала осознанно

Быстрый старт

1. Установить APK
При первом запуске (если есть root) приложение само ставит/обновляет Cheetah_Prop. Обои из комплекта ставятся один раз при первом запуске.

2. Включить LSPosed
LSPosed Manager → Modules → включить модуль приложения (Device ID Spoofer).
Scope: System Framework (android) обязательно для GPS на уровне системы и части telephony.
Добавить приложения, которые должны видеть подмену (Maps, банки, чекеры, Settings…). Вариант «все приложения» проще, но тяжелее.
Перезагрузить устройство (или хотя бы убить целевые процессы после смены scope).
Scope задаётся только в LSPosed Manager. В приложении список можно посмотреть в Scope Manager — только чтение.

3. Активировать лицензию
На Home — кнопка замка / поле кода. Код выдаёт бот. По истечении срока остальные вкладки закрываются.

4. Базовый сценарий «другое устройство»
Hooks → Device — загрузить профили, включить Spoof, выбрать профиль. Перезагрузка желательна (build.prop / fingerprint / kernel).
Hooks → FakerId — включить нужные ID, Generate / Save.
Hooks → SIM / Wi‑Fi и вкладка Location — оператор, вышки, сети, GPS.
Targets — для отдельных пакетов сузить набор хуков, если глобальный профиль ломает приложение.
Help → Advanced — поставить Zygisk/KPM только если нужен hide / native uname / TUN.
Не включайте два независимых спуфера одних и тех же свойств одновременно (чужой Magisk-модуль + Cheetah_Prop на одни и те же ro.*).

Карта интерфейса
Нижняя карусель (без лицензии видны Home и Help):

Home
Среда (root, модуль, LSPosed, GPS, MediaDrm, VPN Hide…), лицензия, перезагрузка, облачный аккаунт, быстрые переходы

Targets
Per-app политика хуков: какие хуки применять в каком пакете

Hide
Скрытие списка приложений (Hide My Applist / Device Hide Zygisk)

VPN Hide
Скрытие VPN/TUN от выбранных приложений

VPN/Proxy
Встроенный туннель / прокси из приложения

Hooks
Хаб: Status, FakerId, SIM, Wi‑Fi, Device (профили), App ID

Location
Auto Spoof GPS, ручные координаты, вышки, спутники, датчики

Camera
StreamDC: подмена камеры (файл / RTMP)

Backup
Облачные бэкапы профилей/настроек

Help
Язык, обновления, Telegram, Advanced (модули и KPM)

Из Home / верхних кнопок также открываются System Info и Scope Manager (список scope LSPosed).

Функции подробно
Home
Индикаторы окружения: root, Cheetah_Prop, LSPosed, GPS spoof, MediaDrm, VPN Hide / Ports, камера.

Лицензия: логин, оставшееся время, ввод кода.

Перезагрузка с подтверждением и обратным отсчётом.

Проверка обновлений (JSON update_check_url → GitHub update.json).

При активной лицензии — переходы в разделы и облако.

Hooks → Status
Сводка мастер-переключателя спуфа, целевая страна (MCC/MNC для SIM/локали), USB / developer hide, карточка батареи. Плитки ведут в подразделы хаба.

Target country подтягивает оператора и связанные prefs под выбранную страну.

Hooks → FakerId

Поштучные идентификаторы. У каждой карточки значение, тумблер и генерация.

Типичный набор:
Android ID, GSF ID, Google Advertising ID, Media DRM ID
Hardware Serial
IMEI / MEID (SIM 1 / 2), IMSI / ICCID, SIM serial, Sub ID, номер
Bluetooth MAC
Locale / time zone (из страны Faker)
Кнопки пачкой: Select all, Cancel all, Random all, Clear, Save / Load list. После Save конфиг пишется в модуль и зеркала. Для свойств, которые читает system_server или init, нужна перезагрузка.

Hooks → SIM
Телефония без «голой» карты: состояние SIM (fake present), число слотов, оператор / carrier id, CellInfo (скрытие или синтетические вышки), сигнал, roaming / тип сети, «натуральный» профиль (conservative / urban / aggressive).
Вышки работают и без реальной сети (как fallback Wi‑Fi), но только в процессах из LSPosed scope. Сохраните настройки — иначе чекеры читают старый XML.

Hooks → Wi‑Fi
Список сохранённых сетей (SSID / BSSID / extras), ручное добавление, генерация 4–7 случайных, редактирование. Скан-результаты и текущая сеть подменяются хуками wifi_*, если включены соответствующие тумблеры (или связанный Wi‑Fi info).

Опционально панель Wigle API для привязки сетей к местности.

Hooks → Device (профили)

Готовые .prop профили (модель, brand, manufacturer, fingerprint, id, tags, kernel, UA, vendor, набор Build.*).

Включить Spoof.

Выбрать профиль — он применяется и поднимается вверх.
Перезагрузить.

Как профиль превращается в build.prop / props модуля

Ядро (Kernel release). Java:
System.getProperty("os.version")
getprop / Runtime.exec (uname -r в оболочке)
Os.uname() на стороне ART, где хук ставится
Native syscall uname() (номер 160 / newuname) LSPosed не перехватывает. Если чекер показывает два разных release — включите Cheetah UnameHide в Help → Advanced → KPM auto loader (нужен APatch/KPatch). KPM читает /data/adb/modules/Cheetah_Prop/device.kernel (зеркало в /data/local/tmp/device.kernel). Свойства os.version / android.kernel.release модуль намеренно не трогает при boot (риск bootloop).

Hooks → App ID
Per-app идентификаторы (свой Android ID и связанные значения на пакет), импорт/поиск приложений. Нужен, когда глобальный Faker ID ломает конкретный клиент.

Targets (per-app хуки)
Глобальный набор хуков + исключения/набор на пакет. Список id совпадает с UI (HookPolicyIds.ALL_META): telephony, Wi‑Fi, IDs, Build, GPS/GNSS, BLE, сенсоры, web fingerprint, USB, батарея, mount shield, airplane, satellite, FLAG_SECURE, boot-lock hide, overlay hide, WebRTC local IP и др.

Инфраструктурные координаторы (FrameworkCentralHooks, PhoneProcessHook, часть VPN Hide в system_server) не выключаются поштучно — они не в этом списке.

Политика пишется в per_app_hooks.json (модуль + зеркала). Хост-менеджер com.motorola.backup хуки на себя не вешает.

Location
Auto Spoof GPS — город по IP, смещение порядка километров, лёгкое движение.
Ручные координаты (карта OSM / ввод).
GeoIP-карточка, высота / точность.
Спутники и созвездия, барометр и движение (чтобы «стоять на месте» не выглядело мёртвым GNSS).
Китайские карты (GCJ-02): хук China map SDK.
BLE: блок скана или синтетические маяки, пока GPS spoof включён (indoor iBeacon).

Скрытие mock location / AppOps.

Для раздачи координат всем приложениям: System Framework в scope + GPS включён + мастер-спуф. Координаты для system_server — файл в каталоге модуля, не tmp.

Hide
Встроенный UI Hide My Applist: приложения, шаблоны (root / xposed / детекторы / Shizuku…), settings templates, логи. Нужен Zygisk-модуль Device Hide Zygisk — ставится из Help → Advanced. После установки — reboot.

VPN Hide и VPN/Proxy

VPN Hide — роли приложений (скрыть интерфейс/сокеты VPN). Zygisk ZIP + опционально Ports ZIP.

VPN/Proxy — свой туннель из приложения; при активном туннеле хук WebRTC прячет локальные ICE IP в scoped-процессах.

vpnhide.kpm не экспортируется (Embed в boot = риск bootloop). Для native TUN используйте cheetah_tunhide.kpm только как Install/Load, никогда Embed.

Camera (StreamDC)
Подмена кадра камеры: файл или RTMP. Отдельный Zygisk-хук из Advanced. Diagnostics — отдельный экран. Overlay управления можно прятать от приложений хуком Hide overlay (scope + тумблер).

Backup
Бэкап/восстановление профилей и настроек (аккаунт на Home).

Help (настройки)
Язык интерфейса (пересоздание Activity).
Telegram-канал, бот, сайт.
Проверка обновления APK.

Advanced (раскрывается)
Install Module — мультивыбор (Cheetah_Prop сюда не входит, он ставится сам при старте):

Модуль, Назначение

Device Hide Zygisk
Скрытие applist (вкладка Hide)

MediaDrm Zygisk
Native DRM id

VPN Hide Zygisk
Скрытие VPN

VPN Hide Ports
Доп. порты

StreamDC Camera
Подмена камеры

Export all KPM → Downloads/kpm. Дальше вручную: KPatch → KPModule → Install/Load.

Файл, Назначение
selinux_hook_1.1.6.1.kpm
SELinux helpers

cheetah_susmap.kpm
Скрытие карт/путей. Только Load, не Embed

cheetah_tunhide.kpm
TUN hide. Только Install/Load

cheetah_unamehide.kpm
Native uname() release

Nohello-….kpm
Антидетект helpers

KPM auto loader — отложенная загрузка после boot (/data/adb/service.d/cheetah_kpm_loader.sh). Флаги: SusMap, TunHide, UnameHide + общий Enable. Никогда Embed в boot image из приложения. Перезагрузка остаётся штатным recovery-путём.

BRENE + SuSFS (KernelSU) — выкладка патча в уже установленный модуль brene. Без BRENE кнопка бесполезна.

Что в scope
GPS «для всех»
android (System Framework) + клиенты карт
SIM / CellInfo / Settings «О телефоне»
целевые чекеры + com.android.settings
LocationManagerService, часть telephony
android, при необходимости com.android.phone
Web fingerprint / UA в Chrome
Chrome (и WebView-хосты)
FLAG_SECURE / скриншоты
android + com.android.systemui

После смены scope перезапустите целевое приложение. Для system_server — reboot.

Предупреждение
Инструмент для своего устройства, отладки и приватности. Обход чужих защит, мошенничество и нарушение закона — на стороне пользователя. Автор не несёт ответственности за такое использование.
