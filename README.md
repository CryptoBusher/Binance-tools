## Пак скриптов для автоматизации некоторых процессов на бирже Binance
Этот пак скриптов предназначен для автоматизации рутинных процессов, связанных с движением денежных средств. Пак будет постоянно пополняться.

Связь с создателем: https://t.me/CrytoBusher <br>
Залетай сюда, чтоб не пропускать дропы подобных скриптов: https://t.me/CryptoKiddiesClub <br>
И сюда, чтоб общаться с крутыми ребятами: https://t.me/CryptoKiddiesChat <br>

## Список скриптов и их применение
1. "01_binance_bulk_withdrawal" - скрипт, с помощью которого можно раскидать крипту на разные кошельки с биржи с разными задержками. Это может быть полезно для мультиаккинга, когда вам нужно раскидать деньги на сотни кошельков и вы не хотите их связывать между собой.
2. "02_wallet_tokens_manager" - скрипт, созданный с целью управления денежными средствами в EVM сетях. На данный момент поддерживается только функция сбора средств с множества кошельков на один в сети Polygon. Для раскидывания средств по кошелькам можно юзать готовые решения, так как такие сервисы не нуждаются в ваших приватных ключах. Со временем буду дополнять функционал и сети.

## Недостатки
1. Скрипт "01_binance_bulk_withdrawal" работает в 1 потоке, так как упор не на скорость.
2. Однопоточность, синхронность. В случае со скриптом "02_wallet_tokens_manager" - была бы полезна асинхронность, буду потом допиливать.
3. Скрипт "02_wallet_tokens_manager" поддерживает только функцию сбора средств с кошельков в сети Polygon. Буду допиливать функционал и добавлять другие сети.

## Логика работы
### 01_binance_bulk_withdrawal
1. Подгружаются кошельки из файла "data/01_wallet_addresses.txt"
2. Происходит коннект к вашему Binance аккаунту
3. Пользователь подтверждает операцию
4. Запускается цикл выводов из биржи. Используются рандомные задержки между выводами и рандомные суммы. Пределы рандомизации указывает пользователь в файле "data/01_config.json"

### 02_wallet_tokens_manager
1. Происходит подключение к сети
2. Пользователь вбивает адрес кошелька, на который будет произведен сбор средств
3. Пользователь указывает Gas Price (GWEI)
4. Пользователь подтверждает операцию
5. Скрипт подтягивает приватные ключи из файла "data/02_private_keys.txt"
6. Запускается цикл для каждого приватного ключа:
   1. Происходит подключение к кошельку
   2. Происходит проверка баланса нативных токенов
   3. Происходит подсчет суммы для отправки за исключением комиссии сети
   4. Происходит проверка суммы (больше нуля или нет)
   5. Происходит отправка транзакции
   6. Цикл повторяется для следующего кошелька

## Первый запуск
### Для всех скриптов
1. Устанавливаем Python (желательно последнюю версию)
2. Качаем репозиторий
3. Открываем терминал, переходим в папку с файлами и пишем команду "pip install -r requirements.txt"

### 01_binance_bulk_withdrawal
1. Открываем файл "data/01_config.json" и вбиваем туда данные по нашему API ключу и, при необходимости, прокси. Ниже расскажу, как достать и настроить API ключ. С прокси я не тестировал работу скрипта, так что жду фидбека. Прокси вписываем (скорее всего) в формате http://логин:пасс@ип:порт. Если нет необходимости юзать прокси (скорее всего ее и не будет) - просто оставьте пустые ковычки.
2. Там же, в файле "data/01_config.json" указываем монету, сеть, минимальную и максимальную сумму (будет рандомизация в этих пределах), минимальную и максимальную задержку в секундах (будет рандомизация в этих пределах).
3. Вбиваем кошельки получателей в файле "data/01_wallet_addresses.txt", каждый с новой строки
4. Открываем скрипт с помощью любого текстового редактора
5. В терминале, находясь в папке проекта, вписываем команду "python 01_binance_bulk_withdrawal.py" и жмем ENTER.
6. Читаем информацию перед стартом в консоли и подтверждаем, вводя в консоль "y" (английская, игрек) и жмем ENTER

### 02_wallet_tokens_manager
1. Открываем файл "data/02_private_keys.txt" и вбиваем туда приватные ключи от кошельков, с которых будем собирать деньги. Каждый с новой строки.
2. В терминале, находясь в папке проекта, вписываем команду "python 02_wallet_tokens_manager.py" и жмем ENTER.
3. Вбиваем в консоль адрес кошелька, на который будем собирать деньги.
4. Вбиваем в консоль Gas Price (GWEI), целое число. Его можно найти в эксплорере (для Polygon - https://polygonscan.com/).
5. Подтверждаем готовность, вбив в консоль "y" (английская буква).

## Как достать и настроить API ключ для работы с Binance
1. Логинимся в свой Binance аккаунт и идем в Account -> API Management

![image](https://user-images.githubusercontent.com/103379312/202818935-a9f68e39-8671-44f9-bc7f-c8a9cbf39cd4.png)

2. Жмем кнопку Create API

![image](https://user-images.githubusercontent.com/103379312/202818989-873c5bba-4d72-448f-880a-a58700b862fb.png)

3. Вбиваем название ключа, которое ни на что не влияет и жмем Next, проходим капчу и верификацию по почте/телефону/2fa

![image](https://user-images.githubusercontent.com/103379312/202819051-b6809194-606f-4e30-ad6e-def3d7b9f824.png)

4. Берем тут API Key + Secret Key для доступа к нашему аккаунту. Никому ни за что их не показываем, ато потеряем все деньги на бирже

![image](https://user-images.githubusercontent.com/103379312/202820402-6d638f79-139c-479a-8e9b-949ba7d33241.png)

5. Жмем Edit restrictions в верхнем правом углу, примерно таким образом выставляем галочки, так же, лучше ограничить доступ только для своего IP, так, в случае, если вы спалите свой API ключ - злоумышленник не сможет получить контроль над вашим аккаунтом.

![image](https://user-images.githubusercontent.com/103379312/202819968-dab54d9b-8bc8-4c2d-ae48-2eef161531d9.png)

6. Жмем Save и подтверждаем действитя по почте/телефону/2fa.
