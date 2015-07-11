# kupon
Тестовый проект для КупиКупон
базовая логика

Как это работает:

1. Пользователь попадает на витрину магазинов, у магазинов есть предложения - офферы. Пользователь переходит по ссылке оффера.
2. В контроллере создаётся сущность LinkVisit, генерируется реферальная ссылка с sub_id партнёра, по которой он попадает на сайт магазина, где осуществляет покупки.
3. От партнёра, в данном случае Admitad, приходит HTTP GET запрос с информацией о подтверждении заказа. 
4. Обработчик ловит этот запрос и создаёт объект сущности Reward согласно правилам, указанным в сущности магазина и оффера, где вычисляется размер вознаграждения по типу вознаграждения, а также время ожидания. Тут же баланс пользователя on_hold увеличивается на сумму вознаграждения. А объект Reward попадает в режим ожидания с записью в поле on_hold_period_end даты и времени окончания режим ожидания.
5. Например с помощью гема ClockWork объекты Reward по скопу on_hold проверяются например раз в час, и те их них, у которых время ожидания подошло, перехотят в статус confirmed, а сумма вознаграждения переходит из баланса в обработке в баланс подтверждённый и доступный к снятию.

Для интеграции с амитад я бы использовал стандартные HTTP запросы и, так как в рамках описанного тестового задания требуется только узнать факт подтвреждения заказа, просто в лоб бы запрограммировал все необхимые методы в его модели, запросы бы отправлял с помощью RestClient.